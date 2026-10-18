# Practice - Model Evaluation

Try these exercises before checking `solution.md`.

## Part A: Concepts
1. Why is training accuracy not a reliable estimate of performance on unseen data?
2. Explain training, validation, and test sets.
3. Define data leakage and give two examples.
4. When is a time-based split better than a random split?
5. Why use group-aware splitting for records from the same customer?
6. Explain accuracy, precision, recall, and F1.
7. What does ROC-AUC measure, and what does it not tell you?
8. Compare MAE, MSE, RMSE, and R².
9. What does cross-validation estimate? What does it not guarantee?
10. Explain overfitting and underfitting.

## Part B: Calculate Classification Metrics
Given TP=42, TN=130, FP=18, FN=10:
11. Calculate accuracy.
12. Calculate precision.
13. Calculate recall.
14. Calculate specificity.
15. Calculate F1.
16. A legitimate transaction is flagged as fraudulent. Which error is this?
17. Why might a spam filter and a medical screening tool prioritize different metrics?

## Part C: Regression Metrics
Actual values: `[3, -0.5, 2, 7]`; predictions: `[2.5, 0, 2, 8]`.
18. Calculate absolute errors and MAE.
19. Calculate MSE and RMSE.
20. Why is RMSE more sensitive to large errors than MAE?
21. What does negative R² mean?
22. Why can MAPE be unsuitable for zero or near-zero targets?

## Part D: Scikit-learn
23. Load Iris and split 80/20 with `random_state=42` and `stratify=y`.
24. Train `LogisticRegression(max_iter=1000)` and report accuracy.
25. Print the confusion matrix and classification report.
26. Compare against `DummyClassifier(strategy="most_frequent")`.
27. Use stratified 5-fold `cross_val_score` on the training partition.
28. Compare `DummyRegressor(strategy="mean")` and `LinearRegression` on the diabetes dataset using MAE and R².
29. Why should preprocessing be inside a pipeline during CV?
30. Why should the final test set not be used repeatedly for model selection?

## Part E: Diagnostics
31. Plot a learning curve and explain a large training-validation gap.
32. Explain how a validation curve helps diagnose model complexity.
33. Compare accuracy, balanced accuracy, F1, ROC-AUC, and average precision for imbalanced data.
34. How would you check for differences across subgroups?
35. Outline a fair comparison of three models using CV and one final test set.

## Part F: Mini-Projects
36. Compare a baseline and two Iris classifiers; include confusion matrix, accuracy, macro-F1, and error analysis.
37. Compare a mean regression baseline with Linear Regression on diabetes data using MAE, RMSE, and R².
38. Design an evaluation plan for rare fraud/security events, including split strategy, metrics, threshold choice, and error costs.
39. Compare shallow and deep decision trees; report training and validation scores and discuss overfitting.
40. Write a stakeholder memo explaining what the score means, remaining errors, limitations, and monitoring needs.

## Checklist
- [ ] Split strategy matches the problem.
- [ ] Baseline included.
- [ ] Metrics fit the task and error costs.
- [ ] Test set untouched during selection.
- [ ] Class-wise errors or residuals inspected.
- [ ] Limitations and uncertainty documented.
