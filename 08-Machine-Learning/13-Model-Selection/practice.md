# Practice: Pipelines

Attempt each exercise before checking `solution.md`. Use Python and scikit-learn. The exercises build from simple sequences to mixed-type workflows and model tuning.

## Beginner

### 1. Explain the value of a pipeline
In your own words, explain three benefits of combining preprocessing and a model in one pipeline.

### 2. Build a basic pipeline
Load the Iris dataset. Create a pipeline with `StandardScaler` followed by `LogisticRegression`. Split the data into train and test sets, fit the pipeline, and report test accuracy.

### 3. Inspect pipeline steps
Create a pipeline using `SimpleImputer(strategy="median")`, `StandardScaler`, and `LogisticRegression`. Print the step names and explain why order matters.

### 4. Prevent data leakage
Describe the problem with this workflow:
```python
X_scaled = StandardScaler().fit_transform(X)
scores = cross_val_score(LogisticRegression(), X_scaled, y, cv=5)
```
Rewrite it using a pipeline.

## Intermediate

### 5. Build a mixed-type preprocessor
Create a DataFrame with numeric columns (`age`, `income`) and categorical columns (`city`, `plan`), including some missing values. Use:
- median imputation and scaling for numeric features
- most-frequent imputation and one-hot encoding for categorical features
- a Logistic Regression classifier as the final estimator

Fit on a training split and evaluate on a test split.

### 6. Unknown categories
Train a pipeline with `OneHotEncoder(handle_unknown="ignore")`. Test it with a row containing a category not seen during training. Explain what this setting does and what it does not guarantee.

### 7. Cross-validation
Evaluate a complete pipeline with `cross_val_score` and stratified 5-fold CV. Report every fold score, mean, and standard deviation.

### 8. Tune model parameters
Use `GridSearchCV` to tune the final Logistic Regression `C` parameter over `[0.01, 0.1, 1, 10]`. Use the parameter name `model__C` if the final step is named `model`. Report best parameters and best CV score.

## Advanced

### 9. Tune preprocessing and model together
Use `GridSearchCV` to compare `StandardScaler` with `MinMaxScaler` while tuning the classifier's `C`. Keep the preprocessor inside the pipeline.

### 10. Feature names
Fit a `ColumnTransformer` containing `OneHotEncoder` and retrieve output feature names using `get_feature_names_out()`. Explain why feature names matter for debugging and model interpretation.

### 11. Save and reload a fitted pipeline
Fit a pipeline, save it with `joblib.dump`, reload it with `joblib.load`, and verify that predictions match. Explain why model files should only be loaded from trusted sources.

### 12. Regression pipeline
Create a regression pipeline with numeric imputation, scaling, and `Ridge`. Evaluate with cross-validation using negative mean absolute error, and convert the result to a positive MAE for reporting.

## Challenge project

### 13. End-to-end customer prediction
Create a small synthetic customer dataset with numeric and categorical features and a binary target.
1. Include missing values in at least two columns.
2. Split the data before fitting preprocessing.
3. Build numeric and categorical preprocessing branches.
4. Combine them with a classifier in a single pipeline.
5. Evaluate with stratified cross-validation.
6. Tune at least one preprocessing parameter and one model parameter.
7. Report the final holdout result.
8. Explain how the pipeline reduces leakage risk and makes inference repeatable.

## Submission checklist

- [ ] Preprocessing is fitted only on training data/folds.
- [ ] Numeric and categorical features receive appropriate transformations.
- [ ] Model evaluation uses an appropriate split strategy and metric.
- [ ] The pipeline is reused consistently for prediction.
- [ ] Hyperparameters are tuned with pipeline parameter names.
- [ ] Final test data is not used for model selection.
