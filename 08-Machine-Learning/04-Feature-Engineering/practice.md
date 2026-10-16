# Practice — Feature Engineering

Attempt each exercise before checking `solution.md`. Use synthetic or approved datasets and explain why a transformation is appropriate rather than applying it automatically.

## Beginner: concepts and simple features

1. Define feature engineering and explain two ways it can help a model.
2. Explain the difference between a feature and the target variable.
3. Create a `total_price` feature from `quantity` and `unit_price`.
4. Create `profit_margin` from `profit` and `revenue`. Handle division by zero safely.
5. Given `birth_date`, describe how you would derive an age feature and what prediction-time issue to consider.
6. A `color` column contains red, blue, and green. Is it nominal or ordinal? Suggest an encoding.
7. A `satisfaction` column contains low, medium, and high. What encoding could preserve its order?
8. Explain why label-encoding unordered categories can mislead some models.
9. Describe when standardization may help and why tree-based models often need less scaling.
10. Compare mean, median, and most-frequent imputation. Give one suitable use case for each.
11. What is an outlier? Give two reasons not to remove every outlier.
12. Explain target leakage with a simple example.

## Intermediate: pandas and scikit-learn

13. Create a `price_per_item` feature from `total_price` and `quantity`, handling zero quantity.
14. From a timestamp column, extract month and day of week using pandas.
15. Create a binary feature `is_weekend` from a date column.
16. Given `visits` and `purchases`, create a conversion-rate feature and handle zero visits.
17. Use `pd.get_dummies` to one-hot encode a small categorical column. State one limitation compared with a fitted encoder in a pipeline.
18. Use `SimpleImputer(strategy="median")` to impute missing numeric values.
19. Apply `StandardScaler`, `MinMaxScaler`, and `RobustScaler` to a sample numeric array. Describe how their outputs differ.
20. Create a log-transformed feature for positive, right-skewed revenue. Explain what happens if revenue is zero or negative.
21. Use `pd.cut` to create meaningful bins for an age feature. Discuss information lost through binning.
22. Use `PolynomialFeatures` to create interaction terms from two numeric features.
23. Flag possible outliers with the IQR method without deleting rows.
24. Build a `ColumnTransformer` that imputes/scales numeric columns and imputes/one-hot encodes categorical columns.
25. Put the preprocessing steps and a logistic regression estimator into a single `Pipeline`.
26. Explain why fitting a scaler or imputer before the train/test split can bias evaluation.
27. Use `SelectKBest` with an appropriate scoring function for a classification problem. Place selection inside a pipeline.
28. Explain how you would handle a category appearing in production but not in training.

## Advanced: design, validation, and trade-offs

29. Create a feature `average_order_value` from transaction-level records. Explain how to aggregate without using future transactions.
30. Design a cyclical encoding for hour of day using sine and cosine. Explain why it may be better than raw hour values.
31. For a high-cardinality `postal_code` column, discuss three options and the risks of each.
32. Explain how to test whether a new feature adds value without overfitting to the test set.
33. Describe a time-aware feature engineering approach for forecasting next month's demand.
34. Explain the difference between feature selection and dimensionality reduction.
35. Explain why feature importance does not imply causation and why correlated features can complicate interpretation.
36. Describe how to test for training-serving skew in engineered features.

## Mini-project: feature engineering for churn prediction

37. Create or use a synthetic customer dataset with numerical and categorical columns and a binary `churn` target.
    - Inspect types, missing values, category counts, and class balance.
    - Add at least three meaningful engineered features.
    - Identify and remove any feature that would not be known at prediction time.
    - Split the data using a reproducible, stratified split.
    - Build a `ColumnTransformer` and `Pipeline`.
    - Compare a simple baseline against logistic regression.
    - Report precision, recall, F1, ROC-AUC, and a confusion matrix.
    - Explain which features may be useful and which assumptions need validation.
    - Document three limitations and a monitoring plan.

38. Write a feature specification for your project. For each feature, record its name, business meaning, source fields, formula, type, missing-value policy, prediction-time availability, and tests.

## Mini-project rubric (100 points)

| Area | Points | Evidence |
|---|---:|---|
| Data understanding | 10 | Schema, missingness, and category checks |
| Feature design | 20 | Relevant features with clear formulas |
| Data leakage prevention | 15 | Prediction-time availability and correct splitting |
| Preprocessing pipeline | 20 | Numeric/categorical steps fitted within pipeline |
| Evaluation | 15 | Baseline comparison and suitable metrics |
| Interpretability | 10 | Clear reasoning and limitations |
| Documentation | 10 | Feature specification and monitoring plan |

## Reflection

- Which engineered feature had the clearest business meaning?
- Which transformation was most sensitive to outliers?
- What information could accidentally leak from the future?
- How would you know if feature distributions changed after deployment?
