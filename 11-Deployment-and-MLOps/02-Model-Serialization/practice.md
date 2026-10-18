# Practice: Model Serialization

Work through these exercises in order. Use the synthetic dataset and pipeline from `examples.ipynb`, or create your own small dataset.

## Exercise 1 — Pickle round trip

1. Train a `LogisticRegression` model.
2. Save it to `artifacts/classifier.pkl` using `pickle`.
3. Load it into a new variable.
4. Compare predictions from the original and loaded model for the same samples.
5. Explain why the artifact should only be loaded if it is trusted.

**Expected outcome:** the two prediction arrays match.

## Exercise 2 — Persist a complete pipeline

1. Build a scikit-learn `Pipeline` with `StandardScaler` and `LogisticRegression`.
2. Fit the pipeline on the training data.
3. Save it as `artifacts/pipeline.joblib`.
4. Load it and predict on held-out samples.
5. Explain why saving the pipeline is safer for consistency than saving only the classifier.

## Exercise 3 — Add metadata

Create `artifacts/metadata.json` with these fields:

- `model_name`
- `model_version`
- `created_utc`
- `python_version`
- `sklearn_version`
- `features`
- `test_accuracy`

Load the JSON file and print the metadata in a readable format. Do not put the model binary itself in JSON.

## Exercise 4 — Smoke test a loaded model

Write a function `load_and_check(path, X_test, expected_features)` that:

1. Loads the saved pipeline with `joblib`.
2. Checks that `feature_names_in_` matches the expected feature names when available.
3. Produces predictions.
4. Checks that the number of predictions equals the number of rows in `X_test`.
5. Returns the loaded model and predictions.

Raise a clear exception when validation fails.

## Exercise 5 — Reproducibility and compatibility

Write a short checklist for a model artifact you would hand to another engineer. Include dependency versions, feature schema, evaluation metrics, artifact checksum or version, trusted storage, and a simple inference test.

## Exercise 6 — Optional challenge: artifact checksum

Calculate a SHA-256 checksum for the serialized artifact using Python's `hashlib`. Explain what the checksum can help detect and what it does **not** prove about the artifact's safety.

## Reflection questions

1. What is the difference between serialization and deserialization?
2. Why is loading a pickle file from an unknown source dangerous?
3. Why can a model saved under one scikit-learn version fail or behave differently under another?
4. What information belongs in metadata rather than inside the model file?
