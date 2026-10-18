# Practice: Advanced Real-World Data Science Projects

Use the synthetic churn-style dataset from `examples.ipynb`, or substitute a properly licensed dataset. Keep your data split and evaluation protocol documented.

## Exercise 1 — Define the decision

Write a one-page problem statement that includes:
- Who will use the model and what action it informs
- Target definition and prediction horizon
- Primary metric and why it matters
- Error costs and operational constraints
- Data privacy, fairness, and other relevant risks

## Exercise 2 — Establish a baseline

1. Train a `DummyClassifier` baseline.
2. Train a logistic regression pipeline with imputation and scaling.
3. Evaluate both on the same held-out split.
4. Report precision, recall, F1, ROC-AUC, and a confusion matrix.
5. Explain whether accuracy alone would be sufficient.

## Exercise 3 — Cross-validation

Use stratified cross-validation on the training data to compare at least two model configurations. Keep the final test set untouched until model selection is complete. Report the mean and standard deviation of your selected metric.

## Exercise 4 — Threshold analysis

For a binary classifier:
1. Generate positive-class probabilities.
2. Compare thresholds such as 0.25, 0.50, and 0.75.
3. Report precision, recall, F1, and the number of predicted positives at each threshold.
4. Explain how the threshold should reflect the cost of false positives and false negatives.

## Exercise 5 — Interpretability and error analysis

Calculate permutation importance on held-out data or a validation split. Inspect examples of false positives and false negatives. Explain why feature importance is not proof of causation and why explanations can be unreliable with correlated features.

## Exercise 6 — Reproducible artifact package

Save:
- A fitted preprocessing/model pipeline
- A JSON metadata file with version, feature names, metrics, and library versions
- A CSV of evaluation metrics
- A short model card with intended use, limitations, and known risks

Ensure the package contains no private data or secrets.

## Exercise 7 — Deployment and monitoring plan

Write a deployment plan describing input validation, latency/error monitoring, drift checks, delayed-label quality monitoring, alert ownership, rollback, and criteria for retraining. Explain why drift alone should not automatically trigger retraining.

## Exercise 8 — Portfolio review

Create a concise report with:
- Problem statement and data provenance
- Baseline and candidate results
- Validation strategy
- Error and segment analysis
- Limitations and ethical considerations
- Instructions to reproduce the results

## Reflection questions

1. Why should preprocessing be inside a cross-validation pipeline?
2. When is a time-based split more appropriate than a random split?
3. Why can a high ROC-AUC coexist with poor precision at a chosen threshold?
4. What does permutation importance measure, and what are its limitations?
5. Why should final test metrics be computed after model selection rather than used during tuning?
