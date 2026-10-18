# Solutions: Model Serialization

These are reference solutions. Run them in the same environment as `examples.ipynb`; exact metric values can vary if you change the data or random seed.

## Exercise 1 — Pickle round trip

```python
from pathlib import Path
import pickle
from sklearn.linear_model import LogisticRegression

artifact_dir = Path("artifacts")
artifact_dir.mkdir(exist_ok=True)

model = LogisticRegression(max_iter=1000, random_state=42)
model.fit(X_train, y_train)

with (artifact_dir / "classifier.pkl").open("wb") as file:
    pickle.dump(model, file)

with (artifact_dir / "classifier.pkl").open("rb") as file:
    restored_model = pickle.load(file)  # Only load trusted files.

original_predictions = model.predict(X_test)
restored_predictions = restored_model.predict(X_test)

assert (original_predictions == restored_predictions).all()
print("Predictions match.")
```

Pickle files can execute code during loading. Never deserialize files from untrusted sources.

## Exercise 2 — Persist a complete pipeline

```python
from pathlib import Path
import joblib
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

artifact_dir = Path("artifacts")
artifact_dir.mkdir(exist_ok=True)

pipeline = Pipeline([
    ("scaler", StandardScaler()),
    ("classifier", LogisticRegression(max_iter=1000, random_state=42)),
])
pipeline.fit(X_train, y_train)

joblib.dump(pipeline, artifact_dir / "pipeline.joblib")
loaded_pipeline = joblib.load(artifact_dir / "pipeline.joblib")  # Trusted artifact only.

predictions = loaded_pipeline.predict(X_test)
assert len(predictions) == len(X_test)
```

The pipeline includes preprocessing and prediction in one object, reducing the risk of applying different transformations during inference.

## Exercise 3 — Add metadata

```python
import json
import platform
from datetime import datetime, timezone
import sklearn
from sklearn.metrics import accuracy_score

metadata = {
    "model_name": "demo-logistic-regression-pipeline",
    "model_version": "1.0.0",
    "created_utc": datetime.now(timezone.utc).isoformat(),
    "python_version": platform.python_version(),
    "sklearn_version": sklearn.__version__,
    "features": list(X_train.columns),
    "test_accuracy": float(accuracy_score(y_test, loaded_pipeline.predict(X_test))),
}

metadata_path = artifact_dir / "metadata.json"
metadata_path.write_text(json.dumps(metadata, indent=2), encoding="utf-8")

loaded_metadata = json.loads(metadata_path.read_text(encoding="utf-8"))
print(json.dumps(loaded_metadata, indent=2))
```

## Exercise 4 — Smoke test a loaded model

```python
import joblib

def load_and_check(path, X_test, expected_features):
    model = joblib.load(path)  # Only load trusted artifacts.
    actual_features = getattr(model, "feature_names_in_", None)

    if actual_features is not None:
        if list(actual_features) != list(expected_features):
            raise ValueError(
                f"Feature mismatch: expected {list(expected_features)}, "
                f"got {list(actual_features)}"
            )

    if hasattr(X_test, "columns") and list(X_test.columns) != list(expected_features):
        raise ValueError("Input DataFrame columns do not match expected feature names.")

    predictions = model.predict(X_test)
    if len(predictions) != len(X_test):
        raise RuntimeError("Prediction count does not match input row count.")

    return model, predictions

checked_model, checked_predictions = load_and_check(
    "artifacts/pipeline.joblib",
    X_test,
    list(X_train.columns),
)
```

If the pipeline was fitted on a NumPy array rather than a DataFrame, feature names may not be available. In that case, maintain and validate the feature schema separately.

## Exercise 5 — Example artifact handoff checklist

- [ ] Model artifact and version are identified.
- [ ] Python and dependency versions are recorded.
- [ ] Input feature names, order, types, and missing-value rules are documented.
- [ ] Validation/test metrics and evaluation date are included.
- [ ] Artifact is stored in access-controlled, trusted storage.
- [ ] Checksum or immutable artifact ID is recorded.
- [ ] A smoke test confirms that loading and inference work.
- [ ] The model was evaluated on data not used for fitting.

## Exercise 6 — SHA-256 checksum

```python
import hashlib
from pathlib import Path

def sha256_file(path):
    digest = hashlib.sha256()
    with Path(path).open("rb") as file:
        for chunk in iter(lambda: file.read(1024 * 1024), b""):
            digest.update(chunk)
    return digest.hexdigest()

print(sha256_file("artifacts/pipeline.joblib"))
```

A checksum can help detect whether bytes changed when compared with a trusted checksum. It does not prove that a file is benign, authentic, or safe to deserialize; an attacker could replace both the file and an untrusted checksum.

## Reflection questions — sample answers

1. Serialization converts an in-memory object into a storable representation; deserialization reconstructs an object from that representation.
2. A malicious pickle can execute code during loading.
3. Internal estimator representations and library behavior can change; scikit-learn does not promise universal cross-version compatibility for persisted models.
4. Keep human-readable metadata such as model version, feature schema, training/evaluation details, metrics, and dependency versions in a sidecar file or model registry.
