# Solutions: Advanced Real-World Data Science Projects

These reference solutions illustrate a sound workflow. They use synthetic data and are not evidence of performance on a real business problem.

## Exercise 1 — Example decision framing

**Decision:** prioritize customers for voluntary retention outreach.

**Target:** whether a customer leaves within a defined future period. The label must be defined before feature engineering, and features must be available at prediction time.

**Metric:** use recall or precision-recall analysis when positive cases are uncommon; incorporate outreach capacity and the cost of false positives/negatives. A probability model may also require calibration.

**Risks:** historical data may encode unequal treatment, and model scores should not be treated as causal evidence that a particular intervention will work.

## Exercise 2 — Baseline and logistic regression

The notebook provides a reusable pipeline and reports precision, recall, F1, ROC-AUC, and a confusion matrix. A compact example:

```python
from sklearn.dummy import DummyClassifier
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import (
    precision_score, recall_score, f1_score, roc_auc_score,
    confusion_matrix
)

baseline = DummyClassifier(strategy="prior")
baseline.fit(X_train, y_train)
baseline_pred = baseline.predict(X_test)
baseline_prob = baseline.predict_proba(X_test)[:, 1]

model = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler()),
    ("classifier", LogisticRegression(max_iter=1000, class_weight="balanced", random_state=42)),
])
model.fit(X_train, y_train)
pred = model.predict(X_test)
prob = model.predict_proba(X_test)[:, 1]

print("Precision:", precision_score(y_test, pred, zero_division=0))
print("Recall:", recall_score(y_test, pred, zero_division=0))
print("F1:", f1_score(y_test, pred, zero_division=0))
print("ROC-AUC:", roc_auc_score(y_test, prob))
print("Confusion matrix:\\n", confusion_matrix(y_test, pred))
```

In practice, fit and tune using training/validation data, and evaluate on the final test set only after selecting the model.

## Exercise 3 — Cross-validation

```python
from sklearn.model_selection import StratifiedKFold, cross_validate
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
import numpy as np

cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
candidate = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler()),
    ("classifier", LogisticRegression(max_iter=1000, class_weight="balanced", random_state=42)),
])
scores = cross_validate(
    candidate, X_train, y_train, cv=cv,
    scoring=["precision", "recall", "f1", "roc_auc"],
)
for metric in ["test_precision", "test_recall", "test_f1", "test_roc_auc"]:
    print(metric, "mean=", scores[metric].mean(), "std=", scores[metric].std())
```

## Exercise 4 — Threshold analysis

```python
import pandas as pd
from sklearn.metrics import precision_score, recall_score, f1_score

probabilities = model.predict_proba(X_test)[:, 1]
rows = []
for threshold in [0.25, 0.50, 0.75]:
    predictions = (probabilities >= threshold).astype(int)
    rows.append({
        "threshold": threshold,
        "precision": precision_score(y_test, predictions, zero_division=0),
        "recall": recall_score(y_test, predictions, zero_division=0),
        "f1": f1_score(y_test, predictions, zero_division=0),
        "predicted_positives": int(predictions.sum()),
    })
pd.DataFrame(rows)
```

A lower threshold usually increases the number of predicted positives, often raising recall and lowering precision. Validate the operating threshold using business costs and a separate validation set rather than tuning on the final test set.

## Exercise 5 — Permutation importance and error analysis

```python
from sklearn.inspection import permutation_importance
import pandas as pd

importance = permutation_importance(
    model, X_test, y_test, scoring="f1",
    n_repeats=10, random_state=42
)
importance_table = pd.DataFrame({
    "feature": X_test.columns,
    "importance_mean": importance.importances_mean,
    "importance_std": importance.importances_std,
}).sort_values("importance_mean", ascending=False)
importance_table
```

Permutation importance measures how much a chosen score changes when a feature is shuffled. Correlated features can substitute for one another, and results depend on the dataset and scoring metric. Importance does not establish causality.

## Exercise 6 — Reproducible artifact package

```python
from pathlib import Path
from datetime import datetime, timezone
import json
import platform
import sklearn
import joblib
from sklearn.metrics import f1_score, roc_auc_score

artifact_dir = Path("artifacts")
artifact_dir.mkdir(exist_ok=True)
joblib.dump(model, artifact_dir / "model.joblib")

metadata = {
    "model_name": "advanced-churn-demo",
    "model_version": "1.0.0",
    "created_utc": datetime.now(timezone.utc).isoformat(),
    "python_version": platform.python_version(),
    "sklearn_version": sklearn.__version__,
    "features": list(X_train.columns),
    "metrics": {
        "f1": float(f1_score(y_test, pred, zero_division=0)),
        "roc_auc": float(roc_auc_score(y_test, prob)),
    },
    "intended_use": "Educational demonstration only",
    "limitations": ["Synthetic data; not validated for real decisions."],
}
(artifact_dir / "metadata.json").write_text(json.dumps(metadata, indent=2), encoding="utf-8")
```

Only load joblib/pickle-based artifacts from trusted sources. For a real project, include the split strategy, data version, threshold, target definition, and known limitations.

## Exercise 7 — Deployment and monitoring plan

- Validate required columns, types, allowed ranges, and missing-value behavior.
- Monitor latency, throughput, errors, and service availability separately from model quality.
- Compare production input distributions with an appropriate reference window.
- Calculate quality metrics only when trustworthy labels arrive.
- Assign alert owners and define severity, investigation, and rollback procedures.
- Retrain only when investigation and evaluation show a justified need; validate candidate models before release.

## Exercise 8 — Report outline

1. Executive summary and decision context
2. Data provenance, permissions, and target definition
3. Cleaning and feature pipeline
4. Baseline and candidate evaluation
5. Validation protocol and threshold selection
6. Error analysis, segment performance, and interpretation
7. Limitations, risks, and intended use
8. Reproduction instructions and artifact references

## Reflection questions — sample answers

1. Putting preprocessing inside the pipeline ensures each validation fold learns transformations only from its training portion, reducing leakage.
2. Use chronological splits when future predictions must be made from past data and temporal structure matters.
3. ROC-AUC measures ranking across thresholds; precision depends on the selected threshold and class prevalence.
4. Permutation importance measures score degradation after shuffling a feature; correlated features and the chosen metric can affect the result.
5. Repeated test-set tuning leaks information into model selection and can make the final estimate optimistic.
