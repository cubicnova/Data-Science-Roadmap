# Solutions: Model Monitoring

These are reference solutions. Drift thresholds are examples for practice only and should be tuned to the use case, sample size, and normal variation.

## Exercise 1 — Input data quality

```python
def quality_report(df):
    numeric = df.select_dtypes(include="number")
    return {
        "rows": int(len(df)),
        "columns": int(df.shape[1]),
        "missing_rate": df.isna().mean().to_dict(),
        "numeric_summary": numeric.describe().to_dict(),
        "duplicate_rows": int(df.duplicated().sum()),
    }

reference_report = quality_report(X_reference)
current_report = quality_report(X_current)
print("Reference:", reference_report)
print("Current:", current_report)

if list(X_reference.columns) != list(X_current.columns):
    raise ValueError("Reference/current schema mismatch")
```

## Exercise 2 — Numeric drift indicator

```python
import numpy as np
import pandas as pd

def standardized_mean_shift(reference, current, epsilon=1e-8):
    rows = []
    for feature in reference.select_dtypes(include="number").columns:
        ref = reference[feature].dropna()
        cur = current[feature].dropna()
        if len(ref) == 0 or len(cur) == 0:
            shift = np.nan
        else:
            shift = abs(cur.mean() - ref.mean()) / max(ref.std(ddof=0), epsilon)
        rows.append({
            "feature": feature,
            "reference_mean": ref.mean() if len(ref) else np.nan,
            "current_mean": cur.mean() if len(cur) else np.nan,
            "standardized_mean_shift": shift,
        })
    return pd.DataFrame(rows)

drift_table = standardized_mean_shift(X_reference, X_current)
drift_table["alert"] = drift_table["standardized_mean_shift"] > 0.5
drift_table
```

The threshold `0.5` is an illustrative setting, not a universal cutoff. This statistic considers mean movement only; it can miss changes in variance, tails, categories, or relationships between features.

## Exercise 3 — Prediction behavior

```python
reference_predictions = model.predict(X_reference)
current_predictions = model.predict(X_current)

reference_share = pd.Series(reference_predictions).value_counts(normalize=True).sort_index()
current_share = pd.Series(current_predictions).value_counts(normalize=True).sort_index()

prediction_comparison = pd.concat(
    [reference_share.rename("reference_share"),
     current_share.rename("current_share")],
    axis=1,
).fillna(0)
prediction_comparison
```

Prediction distributions can change because inputs or the population changed. They do not establish whether predictions are correct; trustworthy labels are needed to evaluate quality.

## Exercise 4 — Quality when labels arrive

```python
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score

def quality_metrics(y_true, predictions):
    return {
        "accuracy": float(accuracy_score(y_true, predictions)),
        "precision": float(precision_score(y_true, predictions, zero_division=0)),
        "recall": float(recall_score(y_true, predictions, zero_division=0)),
        "f1": float(f1_score(y_true, predictions, zero_division=0)),
    }

# Use only when the corresponding current labels are available and aligned.
# current_metrics = quality_metrics(y_current, model.predict(X_current))
```

## Exercise 5 — Create a monitoring report

```python
import json
from datetime import datetime, timezone
from pathlib import Path

report = {
    "timestamp_utc": datetime.now(timezone.utc).isoformat(),
    "window": "simulated-current-batch",
    "reference_rows": int(len(X_reference)),
    "current_rows": int(len(X_current)),
    "reference_missing_rate": X_reference.isna().mean().to_dict(),
    "current_missing_rate": X_current.isna().mean().to_dict(),
    "feature_drift": drift_table.to_dict(orient="records"),
    "notes": "Investigate flagged features before taking action.",
}
Path("artifacts").mkdir(exist_ok=True)
Path("artifacts/monitoring_report.json").write_text(
    json.dumps(report, indent=2), encoding="utf-8"
)
```

## Exercise 6 — Example alert rules

| Alert | Example condition | Response |
|---|---|---|
| Service error rate | Error rate exceeds agreed SLO for a sustained window | Check logs, dependency health, and recent releases |
| Input quality | Required feature missing or schema changed | Pause or quarantine affected batch; contact upstream owner |
| Feature drift | Drift exceeds a calibrated threshold with enough samples | Check collection changes, seasonality, and affected segments |
| Model quality | Labeled-window metric falls below agreed threshold | Validate labels, investigate impact, consider rollback or retraining |

Set actual thresholds, sample sizes, and durations from the service's baseline and business risk.

## Exercise 7 — Example response plan

1. Confirm the alert and sample size; check that the reference/current windows are comparable.
2. Validate schema, missing values, ranges, and upstream data pipelines.
3. Check whether a release, policy, seasonal event, or population shift explains the change.
4. Inspect affected features and important user or business segments.
5. When labels arrive, measure quality and business impact.
6. Decide whether to continue monitoring, mitigate upstream data, roll back, or train and validate a replacement model.
7. Document the decision and follow up on the alert rule.

## Reflection questions — sample answers

1. Data drift is a change in input distribution; concept drift is a change in the relationship between inputs and the target.
2. Latency measures service performance; accuracy measures prediction correctness and usually requires labels.
3. Small samples, seasonality, noisy features, or a poorly calibrated threshold can cause alerts without meaningful degradation.
4. Reliable, correctly aligned ground-truth labels and a defined evaluation window are required.
5. Drift may be benign or caused by data problems; retraining on faulty or unrepresentative data can make the model worse.
