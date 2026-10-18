# Solutions - Hyperparameter Tuning

Reference answers and code. The exact best parameters may vary with the search space and Scikit-learn version.

## Part A - Concepts (1–10)

1. Parameters are learned from training data; hyperparameters are configuration choices set before or around fitting, such as tree depth or regularization strength.
2. Tuning searches for configurations that generalize better according to a chosen validation strategy and metric.
3. Grid search evaluates every specified combination; randomized search samples a fixed number of configurations and can explore a broader space with fewer fits.
4. CV estimates performance over several folds and provides the comparison signal used to select hyperparameters.
5. Repeatedly selecting based on test results adapts decisions to the test set, making the final estimate less independent.
6. A scoring metric defines what “good” means. Select it based on the task and the relative cost of errors.
7. `best_params_` gives the selected configuration; `best_score_` is its mean CV score under the search's scoring rule; `cv_results_` contains results for all tested candidates.
8. Pipeline transformations are fitted separately inside each training fold, reducing leakage and keeping training/prediction transformations consistent.
9. Nested CV uses inner CV for tuning and outer CV for evaluating the full selection procedure. It is useful when a more unbiased estimate is needed and data is limited.
10. Number of candidates, number of CV folds, and fit cost per model; hardware and parallelism also matter.

## Part B - Search Design (11–16)

11. Example:
```python
param_grid = {
    "max_depth": [2, 4, 8, None],
    "min_samples_split": [2, 5, 10],
}
```
This creates 12 configurations before CV.
12. If each of six parameters has five values, a full grid has `5^6 = 15,625` combinations. With five folds, that can mean 78,125 model fits, excluding refit.
13. Randomized search is useful for broad or continuous distributions, large search spaces, or limited compute budgets.
14. A log-spaced range such as `0.001, 0.01, 0.1, 1, 10, 100` explores orders of magnitude rather than only small linear increments.
15. It generally asks Scikit-learn to use all available CPU cores for parallelizable work.
16. Fixed seeds make experiments more reproducible, although results can still differ across library versions or hardware.

## Part C - GridSearchCV (17–23)

```python
import pandas as pd
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split, GridSearchCV, StratifiedKFold
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, classification_report

X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
param_grid = [
    {"C": [0.01, 0.1, 1, 10, 100], "solver": ["lbfgs"]},
]
search = GridSearchCV(
    LogisticRegression(max_iter=2000),
    param_grid=param_grid,
    cv=cv,
    scoring="accuracy",
    n_jobs=-1,
    refit=True,
)
search.fit(X_train, y_train)

print("Best parameters:", search.best_params_)
print("Best CV accuracy:", search.best_score_)
results = pd.DataFrame(search.cv_results_).sort_values("mean_test_score", ascending=False)
display(results[["params", "mean_test_score", "std_test_score", "rank_test_score"]])

pred = search.best_estimator_.predict(X_test)
print("Test accuracy:", accuracy_score(y_test, pred))
print(classification_report(y_test, pred, zero_division=0))
```

The CV score averages results over folds from the training set. The held-out test score is measured on different observations and can differ due to sampling variation and generalization.

## Part D - RandomizedSearchCV (24–29)

```python
from scipy.stats import randint
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split, RandomizedSearchCV, StratifiedKFold
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report, roc_auc_score

X, y = make_classification(
    n_samples=1200, n_features=20, n_informative=8, n_redundant=4,
    weights=[0.8, 0.2], random_state=42
)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

param_distributions = {
    "n_estimators": randint(100, 401),
    "max_depth": [None, 5, 10, 20],
    "min_samples_split": randint(2, 16),
    "min_samples_leaf": randint(1, 8),
    "max_features": ["sqrt", "log2", None],
}
cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
search = RandomizedSearchCV(
    RandomForestClassifier(random_state=42),
    param_distributions=param_distributions,
    n_iter=20,
    scoring="average_precision",
    cv=cv,
    random_state=42,
    n_jobs=-1,
    refit=True,
)
search.fit(X_train, y_train)

print("Best parameters:", search.best_params_)
print("Best CV average precision:", search.best_score_)
pred = search.predict(X_test)
proba = search.predict_proba(X_test)[:, 1]
print(classification_report(y_test, pred, zero_division=0))
print("Test ROC-AUC:", roc_auc_score(y_test, proba))
```

Twenty candidates and five folds imply about 100 candidate-fold fits plus a final refit. A full grid can be far larger depending on how many discrete values are specified.

## Part E - Pipelines and Scoring (30–35)

30. Example:
```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import GridSearchCV, StratifiedKFold

pipe = Pipeline([
    ("scaler", StandardScaler()),
    ("classifier", LogisticRegression(max_iter=2000)),
])
grid = {"classifier__C": [0.001, 0.01, 0.1, 1, 10, 100]}
search = GridSearchCV(
    pipe, grid, cv=StratifiedKFold(5, shuffle=True, random_state=42),
    scoring="accuracy", n_jobs=-1
)
search.fit(X_train, y_train)
```

31. Use a `ColumnTransformer` to impute/scale numeric columns and impute/encode categorical columns, then place it and the classifier in a `Pipeline`. Tune parameters with names such as `classifier__C`. All transformations are fit inside each CV fold.
32. Double underscores separate the pipeline step name from its estimator parameter name.
33. Recall scoring may favor configurations that detect more positives, potentially increasing false positives and lowering precision. Evaluate the confusion matrix and business cost.
34. Accuracy can conceal minority-class failures. Balanced accuracy averages recall across classes; F1 balances positive precision and recall; average precision summarizes precision-recall performance. Choose a primary metric based on the application.
35. Scikit-learn follows “higher is better,” so loss metrics are negated for search. If the best score is `-12`, the corresponding MAE is 12.

## Part F - Mini-Projects (36–40)

36. Use the same split and CV folds for both searches. Record the search space, number of candidate configurations, runtime, best CV score, and final test score. A higher CV score does not guarantee a higher test score.
37. Small `C` means stronger regularization; large `C` means weaker regularization. Compare CV scores across a log-spaced range and inspect the final test performance once.
38. Choose the metric based on the problem, then report supporting metrics and the confusion matrix. Average precision emphasizes the positive class's precision-recall trade-off; it does not specify the operating threshold.
39. Put imputation, scaling, encoding, and classifier inside one pipeline and search the pipeline. Do not pre-fit transforms on all observations before CV.
40. A good tuning report documents the objective, dataset and split, metric, baseline, search space, CV folds, candidate count, compute budget, best parameters, test results, error analysis, and limitations.

## Good Practices
- Choose a metric before tuning.
- Keep the final test set untouched.
- Use pipeline-based preprocessing.
- Use CV appropriate to the data structure.
- Track compute cost and random seeds.
- Report both CV selection results and final test results.
