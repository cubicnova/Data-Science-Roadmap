# Solutions: Cross-Validation

These are reference solutions. Exact scores can vary slightly across scikit-learn versions. Focus on the evaluation design and interpretation, not on matching a particular decimal.

## 1. Why cross-validation?

A single split can be unusually easy or difficult, may not represent the target population, and can produce unstable results on small datasets. The result can also be affected by class imbalance or random variation. Cross-validation evaluates the workflow across several splits and summarizes variability. It does not remove bias caused by leakage, poor sampling, or a mismatch between validation and real deployment.

## 2. Inspect K-Fold

```python
import numpy as np
from sklearn.model_selection import KFold

indices = np.arange(20)
cv = KFold(n_splits=4, shuffle=True, random_state=42)
validation_indices = []

for fold, (train_idx, valid_idx) in enumerate(cv.split(indices), start=1):
    print(f"Fold {fold}: train={train_idx}, validation={valid_idx}")
    validation_indices.extend(valid_idx.tolist())

assert sorted(validation_indices) == indices.tolist()
assert len(validation_indices) == len(set(validation_indices)) == 20
```

Every sample is used for validation exactly once because ordinary K-Fold partitions the samples into disjoint validation folds.

## 3. Classification stratification

```python
import numpy as np
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import KFold, StratifiedKFold

X, y = load_breast_cancer(return_X_y=True)
splitters = {
    "KFold": KFold(n_splits=5, shuffle=True, random_state=42),
    "StratifiedKFold": StratifiedKFold(
        n_splits=5, shuffle=True, random_state=42
    ),
}

for name, splitter in splitters.items():
    print(f"\\n{name}")
    for fold, (_, valid_idx) in enumerate(splitter.split(X, y), start=1):
        print(f"Fold {fold}: {np.bincount(y[valid_idx])}")
```

Stratification aims to preserve approximate class proportions. It is particularly helpful when classes are imbalanced or rare, though it does not solve every sampling or statistical problem.

## 4. Compute F1 scores

```python
from sklearn.datasets import load_breast_cancer
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import StratifiedKFold, cross_val_score
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler

X, y = load_breast_cancer(return_X_y=True)
model = make_pipeline(
    StandardScaler(),
    LogisticRegression(max_iter=2000, random_state=42)
)
cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(model, X, y, cv=cv, scoring="f1")

print("Fold F1:", scores)
print(f"Mean: {scores.mean():.3f}")
print(f"Standard deviation: {scores.std():.3f}")
```

## 5. Multiple metrics and train scores

```python
from sklearn.model_selection import cross_validate

scoring = {
    "accuracy": "accuracy",
    "precision": "precision",
    "recall": "recall",
    "f1": "f1",
}
result = cross_validate(
    model, X, y, cv=cv, scoring=scoring,
    return_train_score=True, n_jobs=-1
)

for metric in scoring:
    test_scores = result[f"test_{metric}"]
    train_scores = result[f"train_{metric}"]
    print(
        f"{metric}: validation={test_scores.mean():.3f} "
        f"± {test_scores.std():.3f}; "
        f"training={train_scores.mean():.3f}"
    )
```

A substantial training-versus-validation gap can indicate overfitting, although the interpretation depends on sample size, metric, and data complexity.

## 6. Compare models fairly

```python
import pandas as pd
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import cross_val_score

models = {
    "Logistic Regression": model,
    "Random Forest": RandomForestClassifier(
        n_estimators=200, random_state=42, class_weight="balanced"
    ),
}
rows = []

for name, estimator in models.items():
    scores = cross_val_score(estimator, X, y, cv=cv, scoring="f1", n_jobs=-1)
    rows.append({
        "model": name,
        "mean_f1": scores.mean(),
        "std_f1": scores.std(),
        "fold_scores": scores.round(3).tolist(),
    })

print(pd.DataFrame(rows).to_string(index=False))
```

Both models use the same folds and metric, making the comparison more meaningful. The Logistic Regression pipeline scales features; a tree-based Random Forest usually does not need scaling.

## 7. Hyperparameter tuning

```python
from sklearn.model_selection import GridSearchCV

search = GridSearchCV(
    estimator=make_pipeline(
        StandardScaler(),
        LogisticRegression(max_iter=2000, random_state=42)
    ),
    param_grid={"logisticregression__C": [0.01, 0.1, 1, 10]},
    scoring="f1",
    cv=cv,
    n_jobs=-1,
    return_train_score=True,
)
search.fit(X, y)

print("Best C:", search.best_params_["logisticregression__C"])
print("Best mean CV F1:", round(search.best_score_, 3))
```

The pipeline is fitted separately within each fold. The best CV score is a model-selection estimate and may be optimistic after extensive tuning; an untouched test set or nested CV can provide a less biased final estimate.

## 8. Prevent leakage

Incorrect: the scaler learns the mean and variance from all observations before folds are created, including validation observations.

Correct:

```python
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import cross_val_score

safe_model = make_pipeline(
    StandardScaler(),
    LogisticRegression(max_iter=2000, random_state=42)
)
scores = cross_val_score(safe_model, X, y, cv=cv, scoring="f1")
```

During each fold, the scaler is fitted only on that fold's training partition.

## 9. Grouped observations

```python
import numpy as np
from sklearn.model_selection import GroupKFold

rng = np.random.default_rng(42)
n_customers = 12
records_per_customer = 3
groups = np.repeat(np.arange(n_customers), records_per_customer)
X_grouped = rng.normal(size=(len(groups), 4))
y_grouped = (X_grouped[:, 0] + rng.normal(size=len(groups)) > 0).astype(int)

cv_group = GroupKFold(n_splits=4)
for fold, (train_idx, valid_idx) in enumerate(
    cv_group.split(X_grouped, y_grouped, groups=groups), start=1
):
    train_groups = set(groups[train_idx])
    valid_groups = set(groups[valid_idx])
    assert train_groups.isdisjoint(valid_groups)
    print(f"Fold {fold}: train groups={sorted(train_groups)}, "
          f"validation groups={sorted(valid_groups)}")
```

The group labels must be passed to `split`. The split is designed to estimate performance on groups not represented in the corresponding training fold.

## 10. Time-series split

```python
import numpy as np
from sklearn.model_selection import TimeSeriesSplit

X_time = np.arange(30).reshape(-1, 1)
cv_time = TimeSeriesSplit(n_splits=4)

for fold, (train_idx, valid_idx) in enumerate(cv_time.split(X_time), start=1):
    print(
        f"Fold {fold}: train ends at {train_idx[-1]}, "
        f"validation starts at {valid_idx[0]}"
    )
    assert train_idx.max() < valid_idx.min()
```

Random K-Fold can train on observations from the future and validate on the past, creating an unrealistic forecast evaluation. Use time-ordered splits and ensure features contain no future information.

## 11. Choose a splitter

- **A: Multiple records per patient:** `GroupKFold` or, for imbalanced classification, `StratifiedGroupKFold`. Keep each patient entirely within one fold.
- **B: Daily demand forecasting:** `TimeSeriesSplit` or a custom rolling-origin validation plan. Respect chronology.
- **C: Independent but imbalanced images:** `StratifiedKFold`, assuming images are genuinely independent. If images come from the same subject/video/session, group-aware splitting may be required.
- **D: Independent house-price records:** `KFold` with shuffling and a fixed seed is a reasonable baseline. If geography, time, or repeated sellers create dependence, adjust the split strategy.

## 12. Reusable evaluation function

This example uses `cross_validate` with F1 and ROC-AUC. For binary classifiers, scikit-learn's `roc_auc` scorer can use decision scores or probabilities as supported by the estimator.

```python
import numpy as np
from sklearn.model_selection import cross_validate

def evaluate_classifier(estimator, X, y, cv):
    result = cross_validate(
        estimator,
        X,
        y,
        cv=cv,
        scoring={"f1": "f1", "roc_auc": "roc_auc"},
        n_jobs=-1,
    )
    return {
        "f1_mean": float(np.mean(result["test_f1"])),
        "f1_std": float(np.std(result["test_f1"])),
        "roc_auc_mean": float(np.mean(result["test_roc_auc"])),
        "roc_auc_std": float(np.std(result["test_roc_auc"])),
    }

report = evaluate_classifier(model, X, y, cv)
print(report)
```

This example assumes binary classification and that each fold can compute both metrics. For multiclass tasks, choose explicit multiclass scorers such as `f1_macro` and a compatible ROC-AUC configuration. Some folds may not contain all classes, making ROC-AUC undefined.

## 13. Challenge project

A strong submission documents the dataset and target, justifies the splitter, uses a leakage-safe pipeline, compares configurations on identical folds, reports fold-level scores plus summary statistics, and explains remaining limitations. In a production-like evaluation, retain a final holdout set that is not used for tuning and evaluate the selected workflow only after decisions are complete.
