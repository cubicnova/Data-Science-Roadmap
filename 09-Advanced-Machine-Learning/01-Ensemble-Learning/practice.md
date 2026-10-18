# Practice — Ensemble Learning

Complete the exercises in order. Use Python, NumPy, pandas, and scikit-learn. Keep the test set untouched until final evaluation, and use cross-validation for model selection.

**Suggested environment:** see `README.md`. Use `random_state=42` where supported for repeatable demonstrations.


## Foundations

1. In your own words, define ensemble learning. Give two reasons a combination of models may generalize better than one model.
2. Explain the bias–variance trade-off and identify which problem bagging is primarily designed to reduce.
3. Compare bagging and boosting in terms of training order, data weighting/resampling, and typical base learner strength.
4. Explain hard voting and soft voting. What capability must each estimator support for soft voting?
5. What is stacking? Explain the role of the meta-model and why out-of-fold predictions matter.
6. Why is an ensemble not guaranteed to outperform its strongest base model? Discuss correlated errors.

## Data preparation

7. Load the Breast Cancer dataset from scikit-learn. Print its shape, class counts, and feature names.
8. Create a stratified 80/20 train/test split with `random_state=42`. Explain why stratification is useful.

## Baseline

9. Train a `DecisionTreeClassifier` baseline and report accuracy, precision, recall, F1, and a confusion matrix.

## Bagging

10. Train a `BaggingClassifier` with decision trees. Compare its cross-validation score with a single tree.

## Random Forest

11. Train a `RandomForestClassifier` with 100 and 400 trees. Compare cross-validation scores and training time.
12. Tune `max_depth`, `min_samples_leaf`, and `max_features`. Explain how each can affect overfitting.

## Extra Trees

13. Train `ExtraTreesClassifier` and compare it with Random Forest using the same folds and metrics.

## Boosting

14. Train an `AdaBoostClassifier`. Vary the number of estimators and describe the observed trend.
15. Train a `GradientBoostingClassifier`. Explore `learning_rate` and `n_estimators` jointly.
16. Describe a situation where boosting may be more sensitive to noise or mislabeled observations than bagging.

## Regression

17. Use `load_diabetes` to compare `RandomForestRegressor` and `GradientBoostingRegressor` using MAE, RMSE, and R².

## Voting

18. Build a hard-voting classifier from logistic regression, a decision tree, and a random forest.
19. Build a soft-voting classifier. Ensure each base estimator supports `predict_proba` and compare with hard voting.
20. Try non-equal voting weights. Explain how weights affect the ensemble and why they should be selected using validation data.

## Stacking

21. Build a `StackingClassifier` using logistic regression, random forest, and gradient boosting, with logistic regression as the final estimator.
22. Compare stacking with its base estimators using the same cross-validation splits.

## Metrics

23. For a class-imbalanced problem, explain why accuracy may be misleading and name three better diagnostics.
24. Generate a classification report and confusion matrix for your strongest ensemble. Interpret false positives and false negatives in context.

## Cross-validation

25. Use `cross_val_score` with stratified 5-fold CV to compare a decision tree, random forest, and gradient boosting.

## Hyperparameter tuning

26. Use `RandomizedSearchCV` or `GridSearchCV` to tune a random forest. Select a metric that matches the task.

## Pipelines

27. Create a `Pipeline` that includes imputation and an ensemble model. Explain how this prevents leakage during CV.

## Interpretability

28. Plot the top 15 Random Forest feature importances. Explain two limitations of impurity-based importance.
29. Use permutation importance on held-out data and compare its ranking with impurity-based importance.

## Robustness

30. Repeat model evaluation across several random seeds or folds. Report mean and standard deviation of the chosen metric.

## Efficiency

31. Compare fit time, prediction time, model size (where practical), and score for two ensemble models.

## Calibration

32. Explain probability calibration. Describe how you would inspect whether a soft-voting model's probabilities are reliable.

## Thresholds

33. Choose a decision threshold based on a stated business cost of false positives and false negatives. Do not tune the threshold on the test set.

## Leakage audit

34. List five ways data leakage can occur in an ensemble workflow and propose a prevention for each.

## Regression tuning

35. Tune `GradientBoostingRegressor` using cross-validation and compare MAE and RMSE.

## Feature selection

36. Compare an ensemble with all features against a feature-selected version using training-only selection.

## Reproducibility

37. Write a short experiment log with dataset, split, random seed, metrics, parameters, and package versions.

## Mini-project

38. Build an end-to-end classification workflow for a built-in dataset: baseline, at least three ensembles, CV comparison, tuning, final test evaluation, and error analysis.
39. Build a regression workflow on the diabetes dataset comparing a linear baseline and at least two ensembles.

## Review

40. Write a short recommendation memo selecting one model for deployment. Include performance, interpretability, latency, maintenance, and risks.

## Submission checklist

- [ ] Code runs from a clean kernel.
- [ ] A simple baseline is included.
- [ ] Cross-validation is used for model selection.
- [ ] The final test set is used only after selecting the approach.
- [ ] Metrics fit the task and class balance.
- [ ] Preprocessing is fitted within a pipeline where appropriate.
- [ ] Results, limitations, and next steps are explained.
