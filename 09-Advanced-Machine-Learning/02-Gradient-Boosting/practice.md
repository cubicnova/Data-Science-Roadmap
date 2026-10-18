# Practice: Gradient Boosting

Complete the exercises in order. Use `examples.ipynb` as a reference, but try each task before checking `solution.md`.

## Exercise 1 — Core Concepts

1. In your own words, explain why gradient boosting trains trees sequentially.
2. Explain the roles of the loss function and learning rate.
3. Give two differences between gradient boosting and random forests.
4. What can happen when `n_estimators` is increased without controlling model complexity?

## Exercise 2 — Classification

Use `sklearn.datasets.load_breast_cancer` and `GradientBoostingClassifier`.

1. Create a train/test split with `test_size=0.2`, `random_state=42`, and stratification.
2. Train a model with `n_estimators=100`, `learning_rate=0.1`, `max_depth=2`, and `random_state=42`.
3. Report accuracy, precision, recall, F1-score, and the confusion matrix.
4. Explain why accuracy alone may not be sufficient in a medical classification setting.

## Exercise 3 — Hyperparameter Comparison

Using the same split as Exercise 2, compare these configurations:

- A: `learning_rate=0.1`, `n_estimators=50`
- B: `learning_rate=0.1`, `n_estimators=150`
- C: `learning_rate=0.03`, `n_estimators=150`

Keep `max_depth=2` and `random_state=42` constant.

1. Compare training and test accuracy for each model.
2. Which model generalizes best according to your chosen metric?
3. Why is it not valid to repeatedly select models based on test-set results in a real project?

## Exercise 4 — Regression

Use `sklearn.datasets.fetch_california_housing` if available, or `make_regression` as a fallback.

1. Split the data into training and test sets.
2. Fit `GradientBoostingRegressor` with `n_estimators=100`, `learning_rate=0.05`, `max_depth=2`, and `random_state=42`.
3. Calculate MAE, RMSE, and \(R^2\).
4. Explain the meaning of each metric and why MAE and RMSE may differ.

## Exercise 5 — Feature Importance

1. Fit a gradient boosting classifier.
2. Display the five features with the highest `feature_importances_`.
3. Explain why feature importance should not be interpreted as proof that a feature causes the outcome.
4. Suggest one additional interpretation method or validation check.

## Exercise 6 — Stochastic Gradient Boosting

1. Train two otherwise comparable models with `subsample=1.0` and `subsample=0.8`.
2. Compare training time and held-out performance.
3. Explain how subsampling can affect variance and bias.
4. Repeat with at least three random seeds and describe the stability of results.

## Exercise 7 — Mini Project

Build a reproducible model-selection experiment.

Requirements:
- Use a stratified train/test split.
- Compare a baseline (e.g. `DummyClassifier`) with at least three gradient boosting configurations.
- Use cross-validation on the training data to choose a configuration.
- Evaluate the selected configuration once on the held-out test set.
- Include a short conclusion describing metrics, trade-offs, limitations, and next steps.

## Submission Checklist

- [ ] Code runs from a clean environment.
- [ ] Random seeds and split strategy are documented.
- [ ] The test set is not used for hyperparameter selection.
- [ ] Metrics are appropriate for the target and class balance.
- [ ] Results and limitations are explained in plain language.
