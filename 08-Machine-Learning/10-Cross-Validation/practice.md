# Practice: Cross-Validation

Try these exercises before opening `solution.md`. Use Python, NumPy, pandas, and scikit-learn. Where requested, explain the reasoning in your own words.

## Beginner

### 1. Why cross-validation?
A model scores 94% accuracy on one train/test split. List at least three reasons this result may not reflect performance on new data. Explain how cross-validation helps.

### 2. Inspect K-Fold
Create 20 sample indices and use `KFold(n_splits=4, shuffle=True, random_state=42)`. Print the train and validation indices for every fold. Verify that every sample appears in a validation fold exactly once.

### 3. Classification stratification
Load the Breast Cancer Wisconsin dataset from `sklearn.datasets`. Compare the class counts in validation folds from `KFold` and `StratifiedKFold`. Why can stratification be useful?

### 4. Compute scores
Use `LogisticRegression` in a `Pipeline` with `StandardScaler`. Evaluate with 5-fold `StratifiedKFold` and `cross_val_score` using F1. Print every fold score, mean, and standard deviation.

## Intermediate

### 5. Multiple metrics
Use `cross_validate` to calculate accuracy, precision, recall, F1, and fit time for a classifier. Return train scores as well. Compare training and validation F1 and explain what a large gap might suggest.

### 6. Compare models fairly
Compare Logistic Regression and Random Forest using the same 5 stratified folds and F1 scoring. Present a small table with mean and standard deviation. Keep all preprocessing inside the pipeline.

### 7. Hyperparameter tuning
Use `GridSearchCV` to tune `C` for Logistic Regression over `[0.01, 0.1, 1, 10]`. Use 5-fold stratified CV and F1 scoring. Report the best parameter and best CV score. Do not use the final test set to select `C`.

### 8. Data leakage
Consider this incorrect workflow:
```python
X_scaled = StandardScaler().fit_transform(X)
scores = cross_val_score(LogisticRegression(), X_scaled, y, cv=5)
```
Explain why it leaks information across folds. Rewrite it using a `Pipeline`.

## Advanced

### 9. Grouped observations
Create a synthetic dataset containing several records for each customer. Use `GroupKFold` to ensure each customer's records stay together. Demonstrate programmatically that the train and validation group sets do not overlap.

### 10. Time-series split
Create 30 time-ordered observations. Use `TimeSeriesSplit(n_splits=4)` and print the last training index and first validation index for each split. Explain why random K-Fold may be inappropriate for forecasting.

### 11. Choose a splitter
For each scenario, recommend a validation strategy and justify your choice:
- A. Predict whether a patient has a condition, with several records per patient.
- B. Forecast next month's demand from historical daily sales.
- C. Classify independent images, with a moderately imbalanced target.
- D. Predict a continuous house price from independent house listings.

### 12. Build an evaluation report
For a classification dataset, create a reusable evaluation function that accepts an estimator, features, labels, and a CV splitter. Return a dictionary containing mean and standard deviation for F1 and ROC-AUC. Handle the fact that some estimators may not expose `predict_proba` by using an appropriate alternative or documenting the limitation.

## Challenge project

### 13. Cross-validation model selection mini-project
Using a public scikit-learn dataset:
1. Define a classification or regression problem.
2. Choose a suitable splitter and at least two metrics.
3. Build a leakage-safe pipeline.
4. Compare at least three model configurations.
5. Report individual fold scores, mean, and standard deviation.
6. Explain why the chosen splitter matches the data.
7. State what additional final test evaluation you would perform in a real project.

## Submission checklist

- [ ] Code runs from a clean notebook/kernel.
- [ ] Splitter is appropriate to the data.
- [ ] Preprocessing is inside a pipeline.
- [ ] Metrics are justified.
- [ ] Results include fold-level scores and summary statistics.
- [ ] Limitations and possible leakage sources are documented.
