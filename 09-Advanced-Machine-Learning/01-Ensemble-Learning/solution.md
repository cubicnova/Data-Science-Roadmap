# Solutions — Ensemble Learning

These are reference approaches, not the only valid answers. Exact scores can vary by scikit-learn version and configuration. Run all comparisons on the same folds and data splits.

## Common imports and data

```python
import numpy as np
import pandas as pd
from sklearn.datasets import load_breast_cancer, load_diabetes
from sklearn.model_selection import train_test_split, StratifiedKFold, KFold, cross_val_score
from sklearn.metrics import (accuracy_score, precision_score, recall_score, f1_score,
                             confusion_matrix, classification_report, mean_absolute_error,
                             mean_squared_error, r2_score)
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import (BaggingClassifier, RandomForestClassifier, ExtraTreesClassifier,
                              AdaBoostClassifier, GradientBoostingClassifier,
                              RandomForestRegressor, GradientBoostingRegressor,
                              VotingClassifier, StackingClassifier)
from sklearn.linear_model import LogisticRegression, Ridge
from sklearn.pipeline import make_pipeline
from sklearn.impute import SimpleImputer
from sklearn.inspection import permutation_importance
from sklearn.model_selection import GridSearchCV, RandomizedSearchCV
```

## 1–6. Foundations

1. Ensemble learning combines multiple estimators. Gains may come from reducing variance, combining complementary decision boundaries, or correcting different model weaknesses.
2. Bias is error from overly restrictive assumptions; variance is sensitivity to training-sample changes. Bagging mainly reduces variance by averaging diverse fits.
3. Bagging trains estimators independently on resampled data and aggregates them. Boosting trains sequentially, with later learners focusing on residuals or examples that remain difficult.
4. Hard voting combines predicted labels. Soft voting averages class probabilities; each base estimator must expose `predict_proba`. Probability quality and scale matter.
5. Stacking trains a second-level model on predictions from base estimators. Out-of-fold predictions prevent the meta-model from learning from in-sample predictions that are unrealistically optimistic.
6. A stronger base model may already capture the signal; ensemble errors may be highly correlated, or extra complexity may increase variance, bias, latency, or overfitting.

## 7–9. Load, split, and baseline

```python
data = load_breast_cancer(as_frame=True)
X, y = data.data, data.target
print(X.shape)
print(y.value_counts())
print(X.columns.tolist())

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.20, stratify=y, random_state=42
)

baseline = DecisionTreeClassifier(random_state=42)
baseline.fit(X_train, y_train)
pred = baseline.predict(X_test)
print("accuracy:", accuracy_score(y_test, pred))
print("precision:", precision_score(y_test, pred))
print("recall:", recall_score(y_test, pred))
print("f1:", f1_score(y_test, pred))
print(confusion_matrix(y_test, pred))
```

Stratification approximately preserves class proportions in both subsets. For medical screening, recall for the positive class may be especially important, but the correct trade-off depends on the stated use case.

## 10–13. Bagging, Random Forest, and Extra Trees

```python
cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
models = {
    "Tree": DecisionTreeClassifier(random_state=42),
    "Bagging": BaggingClassifier(
        estimator=DecisionTreeClassifier(random_state=42),
        n_estimators=100, random_state=42, n_jobs=-1
    ),
    "Random Forest": RandomForestClassifier(
        n_estimators=200, random_state=42, n_jobs=-1
    ),
    "Extra Trees": ExtraTreesClassifier(
        n_estimators=200, random_state=42, n_jobs=-1
    ),
}
for name, model in models.items():
    scores = cross_val_score(model, X_train, y_train, cv=cv, scoring="f1")
    print(name, scores.mean(), scores.std())
```

Use identical folds for fair comparison. More trees often stabilize results but increase compute; improvements eventually diminish. Shallower trees, larger leaf sizes, or fewer candidate features can regularize the forest.

## 14–16. AdaBoost and Gradient Boosting

```python
ada = AdaBoostClassifier(n_estimators=100, learning_rate=0.5, random_state=42)
gb = GradientBoostingClassifier(n_estimators=100, learning_rate=0.05, random_state=42)
for name, model in {"AdaBoost": ada, "Gradient Boosting": gb}.items():
    scores = cross_val_score(model, X_train, y_train, cv=cv, scoring="f1")
    print(name, scores.mean(), scores.std())
```

Treat `learning_rate` and the number of estimators as a joint choice: smaller learning rates commonly require more boosting stages. Boosting can focus heavily on difficult examples, which may include mislabeled data or outliers.

## 17. Regression comparison

```python
reg = load_diabetes(as_frame=True)
Xr, yr = reg.data, reg.target
Xr_train, Xr_test, yr_train, yr_test = train_test_split(
    Xr, yr, test_size=0.20, random_state=42
)
regressors = {
    "Random Forest": RandomForestRegressor(n_estimators=200, random_state=42, n_jobs=-1),
    "Gradient Boosting": GradientBoostingRegressor(random_state=42),
}
for name, model in regressors.items():
    model.fit(Xr_train, yr_train)
    p = model.predict(Xr_test)
    rmse = mean_squared_error(yr_test, p) ** 0.5
    print(name, "MAE", mean_absolute_error(yr_test, p),
          "RMSE", rmse, "R2", r2_score(yr_test, p))
```

Lower MAE/RMSE is better; higher R² is better relative to predicting the target mean. Use the same held-out split for the comparison.

## 18–20. Voting

```python
estimators = [
    ("lr", make_pipeline(SimpleImputer(), LogisticRegression(max_iter=2000)),
    ),
    ("tree", DecisionTreeClassifier(max_depth=4, random_state=42)),
    ("rf", RandomForestClassifier(n_estimators=200, random_state=42, n_jobs=-1)),
]
hard = VotingClassifier(estimators=estimators, voting="hard")
soft = VotingClassifier(estimators=estimators, voting="soft", weights=[2, 1, 2])
for name, model in {"hard": hard, "soft": soft}.items():
    scores = cross_val_score(model, X_train, y_train, cv=cv, scoring="f1")
    print(name, scores.mean(), scores.std())
```

Select weights using cross-validation, not the test set. Voting helps most when models perform reasonably well and make complementary errors.

## 21–22. Stacking

```python
stack = StackingClassifier(
    estimators=[
        ("lr", make_pipeline(SimpleImputer(), LogisticRegression(max_iter=2000))),
        ("rf", RandomForestClassifier(n_estimators=150, random_state=42, n_jobs=-1)),
        ("gb", GradientBoostingClassifier(random_state=42)),
    ],
    final_estimator=LogisticRegression(max_iter=2000),
    cv=5,
    n_jobs=-1
)
scores = cross_val_score(stack, X_train, y_train, cv=cv, scoring="f1")
print(scores.mean(), scores.std())
```

Stacking's internal cross-validation produces out-of-fold predictions for training the final estimator. Keep external evaluation folds separate from all tuning and preprocessing decisions.

## 23–24. Metrics and reports

```python
model = RandomForestClassifier(n_estimators=200, random_state=42, n_jobs=-1)
model.fit(X_train, y_train)
pred = model.predict(X_test)
print(classification_report(y_test, pred, target_names=data.target_names))
print(confusion_matrix(y_test, pred))
```

Accuracy can hide poor minority-class performance. Consider precision, recall, F1, balanced accuracy, and precision-recall AUC. Interpret the confusion matrix using the actual cost of each error.

## 25–27. Cross-validation, tuning, and pipelines

```python
param_grid = {
    "n_estimators": [100, 250],
    "max_depth": [None, 5, 10],
    "min_samples_leaf": [1, 2, 4],
    "max_features": ["sqrt", 0.7],
}
search = GridSearchCV(
    RandomForestClassifier(random_state=42, n_jobs=-1),
    param_grid=param_grid, scoring="f1", cv=cv, n_jobs=-1
)
search.fit(X_train, y_train)
print(search.best_params_, search.best_score_)
best_model = search.best_estimator_
test_pred = best_model.predict(X_test)
print("held-out F1:", f1_score(y_test, test_pred))
```

A pipeline ensures transformations are learned only from the training fold. Example:

```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.pipeline import Pipeline

pipe = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("model", RandomForestClassifier(n_estimators=200, random_state=42))
])
```

Use the pipeline itself in cross-validation and hyperparameter search.

## 28–29. Feature importance

```python
best_model.fit(X_train, y_train)
importance = pd.Series(best_model.feature_importances_, index=X_train.columns)
print(importance.sort_values(ascending=False).head(15))

perm = permutation_importance(
    best_model, X_test, y_test, scoring="f1", n_repeats=10, random_state=42
)
perm_importance = pd.Series(perm.importances_mean, index=X_test.columns)
print(perm_importance.sort_values(ascending=False).head(15))
```

Impurity-based importance can favor high-cardinality features and distribute importance unpredictably among correlated features. Permutation importance measures score degradation when a feature is shuffled, but correlated features can mask one another and test-set interpretation should be done sparingly.

## 30–33. Robustness, efficiency, calibration, and thresholds

30. Repeat stratified CV with multiple seeds or report fold-level scores. Give mean and standard deviation; investigate large variation before deployment.
31. Measure wall-clock fit/predict times under the same hardware and data conditions. Record model size if deployment constraints make it relevant.
32. Compare predicted probabilities with observed frequencies using calibration curves and a proper scoring rule such as Brier score. A high classification score does not guarantee calibrated probabilities.
33. Select a threshold on validation data by minimizing an explicit cost function or meeting a recall/precision requirement. Freeze it before evaluating on the test set.

## 34. Leakage audit

Common leakage sources and prevention:
- Imputing/scaling before splitting → put preprocessing in a pipeline.
- Selecting features using all labels → perform feature selection inside CV.
- Tuning against the test set repeatedly → use training CV and a final untouched test set.
- Duplicate/group-related records across folds → use group-aware splitting.
- Time-dependent records shuffled across the future → use time-aware splits.
- Target-derived features unavailable at prediction time → audit feature availability and data lineage.

## 35–36. Regression tuning and feature selection

```python
from sklearn.ensemble import GradientBoostingRegressor
from sklearn.model_selection import KFold, GridSearchCV

reg_cv = KFold(n_splits=5, shuffle=True, random_state=42)
reg_search = GridSearchCV(
    GradientBoostingRegressor(random_state=42),
    {"n_estimators": [100, 200], "learning_rate": [0.03, 0.05, 0.1],
     "max_depth": [1, 2, 3], "min_samples_leaf": [2, 5]},
    scoring="neg_mean_absolute_error", cv=reg_cv, n_jobs=-1
)
reg_search.fit(Xr_train, yr_train)
print(reg_search.best_params_, -reg_search.best_score_)
```

Feature selection must be learned within each training fold, ideally by placing it in a pipeline. Compare the selected model against the all-feature model using the same folds and metric.

## 37. Reproducibility log

Record: question, dataset/version, target, row/feature counts, split strategy, random seeds, preprocessing, estimator, parameter grid, CV strategy, primary and secondary metrics, test score, runtime, package versions, limitations, and next experiment.

## 38–39. Mini-project outlines

**Classification:** use Breast Cancer or another appropriate dataset; establish a simple baseline; compare at least three ensembles under stratified CV; tune only on training data; report held-out metrics and error analysis; document limitations and leakage controls.

**Regression:** use Diabetes; compare a linear baseline, Random Forest, and Gradient Boosting using MAE/RMSE/R²; tune with CV; evaluate on a held-out test set; discuss residual patterns and deployment constraints.

## 40. Deployment memo rubric

Recommend a model only after considering:
- Predictive performance on representative held-out data.
- Stability across folds/time segments.
- Interpretability and audit requirements.
- Inference latency, memory, and training cost.
- Calibration, fairness, and error consequences where relevant.
- Monitoring, retraining triggers, data drift, and rollback plan.
- Known limitations and evidence needed before production.
