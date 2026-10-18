# Practice: Advanced Feature Engineering

Try each task before consulting `solution.md`. Use fixed random seeds and explain why each transformation is appropriate.

## Exercise 1 — Feature Design

For each scenario, suggest at least two candidate features and explain when they would be available:
1. E-commerce purchase prediction.
2. Customer churn prediction.
3. House-price estimation.
4. Website traffic forecasting.

Identify one feature that could cause leakage if calculated carelessly in each scenario.

## Exercise 2 — Missing Values and Encoding

1. Create a mixed-type DataFrame with missing numeric and categorical values.
2. Impute numeric columns with the median and categorical columns with the most frequent value.
3. One-hot encode categorical columns.
4. Explain why imputers and encoders should be fit on training data only.

## Exercise 3 — Nonlinear and Interaction Features

1. Create a squared feature and an interaction between two numeric features.
2. Compare baseline logistic regression with a model using engineered features.
3. Use a consistent split and metric.
4. Discuss whether performance improved and whether the added complexity is justified.

## Exercise 4 — Date and Time Features

1. Extract month, day of week, hour, and weekend flag from timestamps.
2. Explain how time zones can affect these features.
3. For forecasting, describe how to ensure each feature uses only information available at the forecast timestamp.

## Exercise 5 — Binning and Log Transformation

1. Generate a positive, right-skewed variable.
2. Apply `np.log1p` and visualize both distributions.
3. Create bins with `pd.cut` or `KBinsDiscretizer`.
4. Compare information retained by continuous and binned values.
5. Explain why log transforms cannot be applied blindly to negative values.

## Exercise 6 — Leakage-Safe Pipeline

Using `load_breast_cancer`:
1. Make a stratified train/test split.
2. Build a pipeline with `StandardScaler` and `LogisticRegression`.
3. Run five-fold cross-validation on training data.
4. Fit on all training data and evaluate on the test set once.
5. Explain how the pipeline prevents scaling leakage across folds.

## Exercise 7 — Feature Selection

1. Put `SelectKBest(mutual_info_classif, k=10)` inside a pipeline.
2. Compare cross-validation results against a pipeline without selection.
3. Explain why selection must occur inside cross-validation.
4. Report selected features and discuss stability.

## Exercise 8 — Mini Project

Choose a dataset and build an end-to-end experiment. State the target and prediction time, create a baseline, engineer at least three meaningful features, use a leakage-safe pipeline, compare workflows with cross-validation, evaluate once on a held-out test set, and document assumptions and limitations.

## Submission Checklist

- [ ] Features are justified by domain logic or a testable hypothesis.
- [ ] Transformations work consistently on unseen data.
- [ ] No target leakage or future information is used.
- [ ] Preprocessing is fitted within training folds.
- [ ] Baseline and engineered workflows use comparable evaluation.
- [ ] Results and limitations are documented.
