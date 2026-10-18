# Practice: Experiment Tracking

Use the notebook's synthetic classification dataset or create a dataset of your own. Keep the validation split fixed while comparing model configurations.

## Exercise 1 — Log a baseline

Train a logistic regression classifier and record:

- Model name
- `C` and `max_iter`
- Random seed
- Accuracy, precision, recall, and F1 score
- Python and scikit-learn versions
- A run timestamp

Store the information as a row in a pandas DataFrame and export it to `artifacts/experiment_results.csv`.

## Exercise 2 — Compare configurations

Train at least three logistic regression models with different `C` values, for example `0.1`, `1.0`, and `10.0`.

1. Use the same train/validation split for all runs.
2. Log parameters and metrics for each run.
3. Sort the result table by validation F1 score.
4. Explain why the highest score is not automatically the best production model.

## Exercise 3 — Save artifacts

For the best validation run, save:

- A classification report as a text file.
- A JSON file containing the run parameters and metrics.
- A model artifact using `joblib`.

Store them in a dedicated `artifacts/` directory. Do not store secrets or sensitive records in artifacts.

## Exercise 4 — Add run metadata

Add the following fields to each run record:

- `run_name`
- `data_version`
- `git_commit` (use `"unknown"` if not available)
- `random_state`
- `notes`

Explain how data and code version identifiers improve reproducibility.

## Exercise 5 — Use MLflow (optional)

Install MLflow if it is not already installed.

1. Set an experiment named `experiment-tracking-practice`.
2. Start one MLflow run.
3. Log at least two parameters and two metrics.
4. Log a small artifact such as the JSON metadata file.
5. Open the MLflow UI and locate the run.

Do not log credentials or private data.

## Exercise 6 — Prevent data leakage

Describe a correct approach for comparing hyperparameters using training, validation, and test sets. Explain why repeatedly selecting configurations using test-set performance gives an overly optimistic estimate.

## Exercise 7 — Reproducibility checklist

Create a checklist for handing an experiment to another engineer. Include code revision, data version, dependency versions, random seed, feature schema, parameters, metrics, and artifact location.

## Reflection questions

1. What is the difference between a parameter and a metric?
2. What is an artifact in experiment tracking?
3. Why should model selection generally use validation data rather than the final test set?
4. Why are random seeds useful but not a complete reproducibility guarantee?
5. What kinds of information should never be logged?
