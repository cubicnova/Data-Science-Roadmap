# Practice — Regression

Complete the exercises before consulting `solution.md`. Use Python, pandas, matplotlib, and scikit-learn. Explain the reasoning behind your model and metric choices.

## Beginner: concepts and metrics

1. Define regression and give three examples of continuous targets.
2. Explain the difference between regression and classification.
3. Identify the feature and target in a house-price prediction dataset.
4. Write the simple linear regression equation and explain the intercept and slope.
5. What is a residual? How is it calculated?
6. Explain MAE in plain language when the target is measured in rupees.
7. Compare MAE, MSE, and RMSE. Which is easiest to interpret in target units?
8. What does R² measure? Can R² be negative on test data?
9. Why should a regression model be compared with a baseline?
10. Explain the difference between prediction and causal inference.
11. Name four checks to perform after loading a regression dataset.
12. Describe two reasons why a model may perform poorly on unseen data.

## Intermediate: model building and diagnostics

13. Generate synthetic data with `make_regression` and fit `LinearRegression`.
14. Split data into 80% training and 20% test data with `random_state=42`.
15. Calculate MAE, MSE, RMSE, and R² for a test set.
16. Build a `DummyRegressor(strategy="mean")` baseline and compare it with linear regression.
17. Plot observed versus predicted values and explain what points near the diagonal indicate.
18. Plot residuals versus predicted values. Describe two patterns that would concern you.
19. Fit `Ridge` and `Lasso` models. Explain the purpose of regularization.
20. Why is feature scaling important for Ridge and Lasso?
21. Create polynomial features for a one-feature dataset and compare degree 1, 2, and 5 models.
22. Explain overfitting using training and test errors.
23. Explain multicollinearity and how it can affect coefficient interpretation.
24. Identify when a chronological split is preferable to a random split.
25. Build a preprocessing pipeline for numerical and categorical features, including imputation and one-hot encoding.
26. Explain why preprocessing before the split can cause leakage.
27. Use cross-validation to compare linear regression and Ridge regression.
28. Interpret MAE = 12.5 when the target is delivery duration in minutes.
29. A model has MAE = 4 and RMSE = 15. What might this suggest about its errors?
30. Explain why a high R² does not guarantee reliable predictions for every subgroup.

## Advanced: model evaluation and trade-offs

31. Create a heteroscedastic synthetic dataset where error variance increases with the feature. Plot and explain the residual pattern.
32. Compare Ridge and Lasso using cross-validation. State what must remain fixed for a fair comparison.
33. Describe how to handle a highly skewed positive target. Include one benefit and one risk of a log transformation.
34. Explain why outliers can strongly affect least-squares regression and suggest two diagnostic or robust alternatives.
35. Design a time-series regression evaluation for predicting next month's sales.
36. Explain why feature importance or a large coefficient does not prove causation.
37. Define data leakage in regression and give two examples.
38. Propose a monitoring plan for a deployed regression model when ground-truth labels arrive with a delay.

## Mini-project: house-price regression

39. Use a synthetic or approved housing dataset to predict a continuous price target.
    - Write a problem statement and define when the prediction will be made.
    - Inspect target distribution, missing values, feature types, and suspicious fields.
    - Establish a mean-prediction baseline.
    - Split data reproducibly; use a time/group-aware split if appropriate.
    - Build a preprocessing pipeline for numeric and categorical columns.
    - Compare linear regression with Ridge regression.
    - Report MAE, RMSE, and R² on held-out data.
    - Plot observed vs. predicted values and residuals.
    - Inspect the largest errors and explain possible causes.
    - Document at least three limitations and two monitoring signals.

40. Write a short model card with intended use, target, dataset, split strategy, metrics, known limitations, and monitoring plan.

## Mini-project rubric (100 points)

| Area | Points | Evidence |
|---|---:|---|
| Problem framing | 10 | Clear target and prediction context |
| Data inspection | 10 | Quality checks and leakage review |
| Baseline | 10 | Sensible baseline on the same split |
| Pipeline | 20 | Preprocessing fitted within training workflow |
| Model comparison | 15 | Fair comparison of at least two models |
| Evaluation | 15 | Correct metrics and residual diagnostics |
| Interpretation | 10 | Error analysis and cautious conclusions |
| Reproducibility | 5 | Seed, code, and setup recorded |
| Communication | 5 | Limitations and monitoring documented |

## Reflection

- Which metric best matched your use case, and why?
- What did the residual plot reveal that a single score did not?
- Did regularization change the model's performance or stability?
- What would you monitor after deployment?
