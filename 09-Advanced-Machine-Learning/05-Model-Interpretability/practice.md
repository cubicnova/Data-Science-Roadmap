# Practice: Model Interpretability

Attempt each exercise before consulting `solution.md`. Explain what each result means and what it does **not** prove.

## Exercise 1 — Core Concepts

1. Distinguish intrinsic interpretability from post-hoc explanation.
2. Explain global versus local interpretability with an example.
3. Why is feature importance not the same as causality?
4. Give two limitations of a single global importance ranking.

## Exercise 2 — Permutation Importance

Using `load_breast_cancer` and a fitted Random Forest or Gradient Boosting classifier:

1. Split the dataset into training and test sets with stratification.
2. Fit the model on the training set.
3. Calculate permutation importance on the held-out test set using ROC-AUC.
4. Display the top ten features with mean importance and standard deviation.
5. Repeat with a different metric and explain why the rankings may change.

## Exercise 3 — Correlated Features

1. Create a dataset containing two strongly correlated features and a target related to their shared signal.
2. Fit a model and calculate permutation importance.
3. Explain why the individual importance values may understate the value of either feature.
4. Suggest a way to investigate the shared predictive information.

## Exercise 4 — Partial Dependence

1. Train a tree-based classifier on `load_breast_cancer`.
2. Plot partial dependence for two features.
3. Describe the general trend without making causal claims.
4. Identify one reason the plot may be misleading when features are correlated.

## Exercise 5 — Local Explanation

1. Choose one test row and obtain its predicted class and probability.
2. Explain what a local explanation attempts to communicate.
3. If SHAP is installed, use `shap.Explainer` or a suitable model-specific explainer.
4. Describe the baseline output and the largest positive and negative contributions.
5. Explain why the result describes the model rather than the underlying causal process.

## Exercise 6 — Explanation Stability

1. Fit comparable models using at least three random seeds or cross-validation folds.
2. Calculate permutation importance for each.
3. Compare top-feature overlap and importance variability.
4. Explain how instability should affect the confidence of your conclusions.

## Exercise 7 — Communicating Results

Write a short, non-technical summary of a model interpretation report. Include:
- the model and evaluation sample;
- the most important features;
- one local explanation;
- two limitations;
- one recommended next check.

Avoid unsupported causal statements.

## Exercise 8 — Mini Project

Choose a public or built-in dataset and produce an interpretation report.

Requirements:
- State the prediction target and evaluation metric.
- Report held-out performance.
- Provide global importance using permutation importance.
- Include a partial-dependence plot or a suitable alternative.
- Explain one individual prediction.
- Check for correlation or explanation instability.
- Document limitations and avoid treating explanations as causal evidence.

## Submission Checklist

- [ ] Explanations are based on held-out data where appropriate.
- [ ] Metrics and model outputs are clearly defined.
- [ ] Correlated features and sampling uncertainty are discussed.
- [ ] Local explanations are not generalized to all predictions.
- [ ] Causal claims are avoided unless justified independently.
