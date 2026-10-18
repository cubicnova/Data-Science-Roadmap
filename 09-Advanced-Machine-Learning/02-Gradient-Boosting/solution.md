# Solutions: Gradient Boosting

These are reference solutions. Small numerical differences can occur across scikit-learn versions. Focus on the method and interpretation rather than matching a particular score exactly.

## Exercise 1 — Core Concepts

1. Boosting is sequential because each stage is added to the current ensemble to reduce its remaining loss. The next learner depends on the predictions made by earlier learners.
2. The loss function measures prediction error in a way suited to the task. The learning rate scales each tree's contribution; a smaller value makes each update more conservative and often requires more trees.
3. Gradient boosting adds learners sequentially to reduce loss; random forests train trees independently and aggregate their predictions. Gradient boosting often needs more careful tuning of learning rate and stage count.
4. More estimators may improve fit initially, but can increase computation and may overfit, especially when trees are deep or the learning rate is high.

## Exercise 2 — Classification

```python
from sklearn.datasets import load_breast_cancer
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import (
    accuracy_score, precision_score, recall_score, f1_score,
    confusion_matrix, classification_report
)

data = load_breast_cancer()
X, y = data.data, data.target

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

model = GradientBoostingClassifier(
    n_estimators=100, learning_rate=0.1,
    max_depth=2, random_state=42
)
model.fit(X_train, y_train)
pred = model.predict(X_test)

print("Accuracy:", accuracy_score(y_test, pred))
print("Precision:", precision_score(y_test, pred))
print("Recall:", recall_score(y_test, pred))
print("F1:", f1_score(y_test, pred))
print(confusion_matrix(y_test, pred))
print(classification_report(y_test, pred, target_names=data.target_names))
```

The positive class in this dataset is `1` (`benign`), so the default binary precision/recall refer to benign. If the clinical priority is detecting malignant cases, explicitly choose the malignant label as positive or report per-class metrics. A false negative can be much more costly than a false positive, so accuracy alone is inadequate.

## Exercise 3 — Hyperparameter Comparison

```python
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.metrics import accuracy_score

configs = {
    "A": {"learning_rate": 0.1, "n_estimators": 50},
    "B": {"learning_rate": 0.1, "n_estimators": 150},
    "C": {"learning_rate": 0.03, "n_estimators": 150},
}

for name, params in configs.items():
    model = GradientBoostingClassifier(
        **params, max_depth=2, random_state=42
    )
    model.fit(X_train, y_train)
    print(
        name,
        "train accuracy =", accuracy_score(y_train, model.predict(X_train)),
        "test accuracy =", accuracy_score(y_test, model.predict(X_test))
    )
```

Use the same split and report both train and test results. A model with very high training performance but weaker test performance may be overfitting. Do not assume a configuration is best until you examine the actual metrics. In production, use cross-validation on training data for selection and reserve the test set for one final unbiased estimate.

## Exercise 4 — Regression

```python
import numpy as np
from sklearn.datasets import make_regression
from sklearn.ensemble import GradientBoostingRegressor
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score

X, y = make_regression(
    n_samples=1500, n_features=10, noise=15, random_state=42
)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

model = GradientBoostingRegressor(
    n_estimators=100, learning_rate=0.05,
    max_depth=2, random_state=42
)
model.fit(X_train, y_train)
pred = model.predict(X_test)

mae = mean_absolute_error(y_test, pred)
rmse = np.sqrt(mean_squared_error(y_test, pred))
r2 = r2_score(y_test, pred)

print("MAE:", mae)
print("RMSE:", rmse)
print("R²:", r2)
```

- **MAE** is the average absolute prediction error, in target units.
- **RMSE** is also in target units but penalizes larger errors more strongly.
- **R²** compares the model against predicting the target mean; it can be negative on held-out data.

The synthetic fallback makes the exercise reproducible and avoids requiring a network download. If using California Housing, note the dataset's source and licensing and keep a separate test set.

## Exercise 5 — Feature Importance

```python
import pandas as pd

importance = pd.Series(
    model.feature_importances_,
    index=data.feature_names
).sort_values(ascending=False)

print(importance.head(5))
```

Run this with the classifier from Exercise 2, not the regression model above, so `data.feature_names` matches the model's features. Tree impurity importance can be biased toward features with many possible split points and does not show causal effect. Consider permutation importance on held-out data or SHAP values, and check whether conclusions remain stable across folds.

## Exercise 6 — Stochastic Gradient Boosting

```python
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.metrics import f1_score
import time

for subsample in [1.0, 0.8]:
    start = time.perf_counter()
    model = GradientBoostingClassifier(
        n_estimators=100, learning_rate=0.1, max_depth=2,
        subsample=subsample, random_state=42
    )
    model.fit(X_train, y_train)
    elapsed = time.perf_counter() - start
    pred = model.predict(X_test)
    print(
        "subsample =", subsample,
        "F1 =", f1_score(y_test, pred),
        "seconds =", round(elapsed, 3)
    )
```

Subsampling introduces randomness and can reduce variance, but it may increase bias. Runtime and score differences are dataset- and machine-dependent. Repeat over several seeds to avoid drawing conclusions from one run.

## Exercise 7 — Mini Project

Example workflow:

```python
from sklearn.dummy import DummyClassifier
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.model_selection import StratifiedKFold, cross_val_score
from sklearn.metrics import classification_report

cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)

candidates = {
    "baseline": DummyClassifier(strategy="most_frequent"),
    "gb_50": GradientBoostingClassifier(
        n_estimators=50, learning_rate=0.1, max_depth=2, random_state=42
    ),
    "gb_100": GradientBoostingClassifier(
        n_estimators=100, learning_rate=0.05, max_depth=2, random_state=42
    ),
    "gb_shallow": GradientBoostingClassifier(
        n_estimators=150, learning_rate=0.03, max_depth=1, random_state=42
    ),
}

for name, estimator in candidates.items():
    scores = cross_val_score(
        estimator, X_train, y_train, cv=cv, scoring="f1"
    )
    print(name, "mean CV F1:", scores.mean(), "std:", scores.std())

# After selecting a configuration from CV, fit it on all training data,
# then evaluate once on X_test/y_test.
```

A good report includes the chosen metric, mean and spread of cross-validation scores, final test metrics, baseline comparison, possible class imbalance, and limitations. Do not report only the single best fold.
