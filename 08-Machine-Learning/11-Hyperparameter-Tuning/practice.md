# Practice - Hyperparameter Tuning

Attempt the exercises before checking `solution.md`. Use Python and Scikit-learn for coding tasks.

## Part A - Concepts (1–10)

1. Explain the difference between model parameters and hyperparameters.
2. Why do we tune hyperparameters?
3. Compare `GridSearchCV` and `RandomizedSearchCV`.
4. What is the role of cross-validation during hyperparameter tuning?
5. Why must the final test set remain untouched during tuning?
6. What is a scoring metric, and why should it be chosen before searching?
7. Explain the purpose of `best_params_`, `best_score_`, and `cv_results_`.
8. Why should preprocessing be included in a `Pipeline` during cross-validation?
9. What is nested cross-validation, and when can it be useful?
10. Name three factors that determine the computational cost of a search.

## Part B - Search Design (11–16)

11. Suggest a small grid for tuning `DecisionTreeClassifier` with `max_depth` and `min_samples_split`.
12. Explain why a grid with many values across six hyperparameters can become expensive.
13. When might randomized search be preferable to exhaustive grid search?
14. Why is a log-spaced range often useful for Logistic Regression's `C` parameter?
15. What does `n_jobs=-1` generally request in Scikit-learn search tools?
16. Why should random seeds be fixed where supported?

## Part C - GridSearchCV (17–23)

Use the Iris dataset and Logistic Regression.

17. Load Iris and create a stratified 80/20 train/test split.
18. Create a grid for `C` and `solver` using valid combinations.
19. Run `GridSearchCV` with stratified 5-fold cross-validation and accuracy scoring.
20. Print `best_params_` and `best_score_`.
21. Inspect `cv_results_` as a pandas DataFrame and sort by mean test score.
22. Evaluate the best estimator on the held-out test set.
23. Explain why the test score may differ from the best cross-validation score.

## Part D - RandomizedSearchCV (24–29)

24. Create a reproducible binary dataset with `make_classification`.
25. Split it with stratification.
26. Define a parameter distribution for `RandomForestClassifier`.
27. Run `RandomizedSearchCV` with a fixed `n_iter`, `random_state`, and CV strategy.
28. Report the selected parameters, CV score, and final test metrics.
29. Compare the approximate search cost of randomized search with a full grid.

## Part E - Pipelines and Scoring (30–35)

30. Build a pipeline containing `StandardScaler` and `LogisticRegression`, then tune `classifier__C`.
31. Build a mixed-type preprocessing pipeline with `ColumnTransformer` and tune a classifier parameter.
32. Explain why a parameter for a pipeline step uses a double-underscore name.
33. Tune a classifier using `scoring="recall"` and explain the trade-off that may result.
34. For an imbalanced dataset, compare `accuracy`, `balanced_accuracy`, `f1`, and `average_precision`.
35. Use `scoring="neg_mean_absolute_error"` for regression and explain why Scikit-learn returns a negative score.

## Part F - Mini-Projects (36–40)

36. **Grid vs. random search:** tune a decision tree with both methods and compare CV scores, runtime, and test score.
37. **Regularization tuning:** tune Logistic Regression's `C` over a log-spaced range. Explain the effect of stronger and weaker regularization.
38. **Imbalanced classification:** tune a model for average precision or recall. Report precision, recall, F1, confusion matrix, and the limitations of the chosen metric.
39. **Leakage-safe preprocessing:** construct a pipeline with missing-value imputation, scaling, and a classifier. Tune it without fitting preprocessing on the full dataset.
40. **Tuning report:** document the task, data split, metric, search space, CV strategy, compute budget, best parameters, baseline, final test results, and limitations.

## Submission Checklist

- [ ] Final test set separated before tuning.
- [ ] Metric matches the task.
- [ ] CV strategy matches the data structure.
- [ ] Preprocessing is inside the pipeline.
- [ ] Search space and compute budget are justified.
- [ ] Baseline and final test performance reported.
- [ ] Reproducibility and limitations documented.
