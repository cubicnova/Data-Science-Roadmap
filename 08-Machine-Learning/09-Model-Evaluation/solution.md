# Solutions - Model Evaluation

Reference answers and code. Your exact scores may vary slightly by library version.

## Part A: Concepts
1. Models can memorize training examples or noise; training scores are often optimistic.
2. Training fits parameters; validation supports model/threshold selection; test data gives the final held-out estimate.
3. Leakage is use of information unavailable at prediction time. Examples: scaling before splitting, using future features, or placing records from the same entity in both partitions when that is not representative of deployment.
4. Use time-based splitting when predicting future outcomes from past observations.
5. Related records may share entity-specific patterns, causing an unrealistically easy test set.
6. Accuracy is overall correctness; precision is correctness among predicted positives; recall is detection among actual positives; F1 balances precision and recall.
7. ROC-AUC measures ranking across thresholds; it does not choose a threshold, measure calibration, or tell the precision at a particular threshold.
8. MAE averages absolute errors; MSE averages squared errors; RMSE is the square root of MSE; R² compares with a mean-prediction reference and can be negative.
9. CV estimates performance across folds and gives a sense of variability; it cannot fix leakage, bad labels, or distribution shift.
10. Overfitting: training much better than validation. Underfitting: both are poor.

## Part B: Metric Calculations
Given TP=42, TN=130, FP=18, FN=10, total=200:
- Accuracy = (42+130)/200 = **0.86**
- Precision = 42/(42+18) = **0.70**
- Recall = 42/(42+10) = **0.8077**
- Specificity = 130/(130+18) = **0.8784**
- F1 = 2TP/(2TP+FP+FN) = 84/112 = **0.75**
- A legitimate transaction flagged as fraud is a **false positive**.
- Spam filters may prioritize precision to avoid losing legitimate mail; screening may prioritize recall to reduce missed positive cases while tracking false-positive burden.

## Part C: Regression Calculations
Absolute errors: `[0.5, 0.5, 0, 1]`; MAE = **0.5**.
Squared errors: `[0.25, 0.25, 0, 1]`; MSE = **0.375**; RMSE = √0.375 ≈ **0.6124**.
Squaring magnifies the relative contribution of large errors. Negative R² means performance is worse than predicting the evaluation-set mean. MAPE divides by actual values and becomes undefined at zero and unstable near zero.

## Part D: Scikit-learn Examples

### Iris classification and cross-validation
```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split, cross_val_score, StratifiedKFold
from sklearn.linear_model import LogisticRegression
from sklearn.dummy import DummyClassifier
from sklearn.metrics import accuracy_score, confusion_matrix, classification_report

iris = load_iris()
X, y = iris.data, iris.target
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

model = LogisticRegression(max_iter=1000)
model.fit(X_train, y_train)
pred = model.predict(X_test)
print("Accuracy:", accuracy_score(y_test, pred))
print(confusion_matrix(y_test, pred))
print(classification_report(y_test, pred, target_names=iris.target_names, zero_division=0))

baseline = DummyClassifier(strategy="most_frequent")
baseline.fit(X_train, y_train)
print("Baseline accuracy:", accuracy_score(y_test, baseline.predict(X_test)))

cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(LogisticRegression(max_iter=1000), X_train, y_train,
                         cv=cv, scoring="accuracy")
print("Fold scores:", scores)
print("Mean:", scores.mean(), "Std:", scores.std())
```

CV above uses only the training partition. Keep the test partition separate for final evaluation.

### Regression comparison
```python
import numpy as np
from sklearn.datasets import load_diabetes
from sklearn.model_selection import train_test_split
from sklearn.dummy import DummyRegressor
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score

X, y = load_diabetes(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
for name, model in {
    "Mean baseline": DummyRegressor(strategy="mean"),
    "Linear Regression": LinearRegression(),
}.items():
    model.fit(X_train, y_train)
    pred = model.predict(X_test)
    print(name, {
        "MAE": mean_absolute_error(y_test, pred),
        "RMSE": np.sqrt(mean_squared_error(y_test, pred)),
        "R2": r2_score(y_test, pred),
    })
```

Put learned preprocessing inside a `Pipeline` during CV so each fold fits transformations only on its training portion. Repeated test-set tuning weakens the independence of the final estimate.

## Part E: Diagnostics
31. A large persistent training-validation gap can indicate overfitting/high variance. Poor performance on both can indicate underfitting, noisy labels, or weak features.
32. A validation curve compares training and validation scores over hyperparameter values, revealing when added complexity stops helping generalization.
33. Accuracy may hide minority-class failures. Balanced accuracy averages recall across classes; F1 balances positive-class precision/recall; ROC-AUC measures ranking; average precision summarizes precision-recall behavior. Choose a primary metric based on the application and report supporting metrics.
34. Evaluate relevant subgroups separately, include sample sizes and uncertainty, inspect error rates and data quality, and avoid conclusions from tiny groups.
35. Use the same split/CV folds, preprocessing pipelines, baseline, metrics, and selection rule for each model. Select using training/CV, then evaluate the chosen approach once on the untouched test set.

## Part F: Mini-Project Guidance
36. Include a baseline, two models, confusion matrix, accuracy, macro-F1, class-specific errors, and limitations of the small educational dataset.
37. Compare baseline and Linear Regression on the same held-out rows; report MAE, RMSE, R², and interpret the error in target units.
38. Use time- or group-aware splitting if appropriate. Report precision, recall, F1, PR-AUC/average precision, ROC-AUC, confusion matrix, and threshold rationale.
39. Example:
```python
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier

X, y = load_breast_cancer(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)
for depth in [1, 3, None]:
    model = DecisionTreeClassifier(max_depth=depth, random_state=42)
    model.fit(X_train, y_train)
    print(depth, model.score(X_train, y_train), model.score(X_test, y_test))
```
For formal selection, compare with CV and reserve test data for final evaluation.
40. A stakeholder memo should describe the metric, test population, baseline comparison, main errors, uncertainty, limitations, expected distribution shift, and monitoring plan.

## Good Practices
- Match the split to deployment.
- Keep preprocessing/resampling inside the training workflow.
- Compare with a baseline.
- Report uncertainty and relevant subgroup results.
- Distinguish ranking, threshold-dependent, and calibration metrics.
