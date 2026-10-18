# Solutions: Experiment Tracking

These are reference solutions. Metric values are produced by the code and may differ if you change the data, library versions, or split.

## Exercise 1 — Log a baseline

```python
from pathlib import Path
from datetime import datetime, timezone
import platform
import pandas as pd
import sklearn
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score

artifact_dir = Path("artifacts")
artifact_dir.mkdir(exist_ok=True)

model = LogisticRegression(C=1.0, max_iter=1000, random_state=42)
model.fit(X_train, y_train)
predictions = model.predict(X_val)

record = {
    "run_name": "logistic-regression-baseline",
    "model": "LogisticRegression",
    "C": 1.0,
    "max_iter": 1000,
    "random_state": 42,
    "accuracy": accuracy_score(y_val, predictions),
    "precision": precision_score(y_val, predictions, zero_division=0),
    "recall": recall_score(y_val, predictions, zero_division=0),
    "f1": f1_score(y_val, predictions, zero_division=0),
    "python_version": platform.python_version(),
    "sklearn_version": sklearn.__version__,
    "timestamp_utc": datetime.now(timezone.utc).isoformat(),
}
results = pd.DataFrame([record])
results.to_csv(artifact_dir / "experiment_results.csv", index=False)
results
```

## Exercise 2 — Compare configurations

```python
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score

records = []
for c_value in [0.1, 1.0, 10.0]:
    model = LogisticRegression(C=c_value, max_iter=1000, random_state=42)
    model.fit(X_train, y_train)
    predictions = model.predict(X_val)
    records.append({
        "run_name": f"logreg-C-{c_value}",
        "C": c_value,
        "accuracy": accuracy_score(y_val, predictions),
        "precision": precision_score(y_val, predictions, zero_division=0),
        "recall": recall_score(y_val, predictions, zero_division=0),
        "f1": f1_score(y_val, predictions, zero_division=0),
    })

comparison = pd.DataFrame(records).sort_values("f1", ascending=False)
comparison
```

A slightly higher score may be within normal validation variation. Also consider latency, calibration, fairness, interpretability, operational cost, and the metric that best reflects the use case.

## Exercise 3 — Save artifacts

```python
import json
import joblib
from sklearn.metrics import classification_report

best_c = float(comparison.iloc[0]["C"])
best_model = LogisticRegression(C=best_c, max_iter=1000, random_state=42)
best_model.fit(X_train, y_train)
best_predictions = best_model.predict(X_val)

(artifact_dir / "classification_report.txt").write_text(
    classification_report(y_val, best_predictions, zero_division=0),
    encoding="utf-8",
)
joblib.dump(best_model, artifact_dir / "best_model.joblib")

metadata = {
    "run_name": f"logreg-C-{best_c}",
    "parameters": {"C": best_c, "max_iter": 1000, "random_state": 42},
    "metrics": {
        "accuracy": float(accuracy_score(y_val, best_predictions)),
        "f1": float(f1_score(y_val, best_predictions, zero_division=0)),
    },
}
(artifact_dir / "best_run.json").write_text(
    json.dumps(metadata, indent=2), encoding="utf-8"
)
```

Only load model artifacts from trusted sources; `joblib` uses pickle-based persistence.

## Exercise 4 — Add run metadata

```python
record.update({
    "data_version": "synthetic-v1",
    "git_commit": "unknown",
    "notes": "Baseline validation run",
})
```

Data and code identifiers help establish which inputs and implementation produced a result.

## Exercise 5 — MLflow (optional)

```python
import mlflow

mlflow.set_experiment("experiment-tracking-practice")

with mlflow.start_run(run_name="logreg-baseline"):
    mlflow.log_param("model", "LogisticRegression")
    mlflow.log_param("C", 1.0)
    mlflow.log_metric("validation_accuracy", float(accuracy_score(y_val, predictions)))
    mlflow.log_metric("validation_f1", float(f1_score(y_val, predictions, zero_division=0)))
    mlflow.log_artifact(str(artifact_dir / "best_run.json"))
```

The artifact must exist before `mlflow.log_artifact` is called. Start the UI in a terminal using the tracking store configured for your run, for example `mlflow ui --backend-store-uri ./mlruns`.

## Exercise 6 — Prevent data leakage

Fit candidate models using the training split, choose settings using the validation split (or cross-validation within the training data), and evaluate the selected approach once on the held-out test set. Repeatedly tuning against test results leaks information from the test set into model selection and makes the reported test score less independent.

## Exercise 7 — Example reproducibility checklist

- [ ] Code revision or commit recorded
- [ ] Dataset version or stable data identifier recorded
- [ ] Python and dependency versions recorded
- [ ] Random seed and split strategy recorded
- [ ] Feature names, order, types, and preprocessing documented
- [ ] Parameters and metrics logged
- [ ] Model and reports stored at a known artifact location
- [ ] No secrets or sensitive records logged
- [ ] Loaded model passes a smoke test

## Reflection questions — sample answers

1. A parameter is a configuration value used by a run; a metric is a measured result.
2. An artifact is a file generated or retained by a run, such as a model, report, plot, or metadata file.
3. Validation data supports model selection; the test set should provide a final, less-biased evaluation.
4. Seeds help make random operations repeatable, but library versions, hardware, parallelism, and nondeterministic operations can still affect results.
5. Do not log passwords, access tokens, private keys, or sensitive personal or customer records.
