# Reference Solutions — Feature Engineering

These are reference approaches. A different transformation may be appropriate when supported by the data, domain knowledge, and validation results.

## Beginner: concepts and simple features

### 1. Definition
Feature engineering transforms raw fields into representations that help a model learn. It can expose meaningful relationships (such as ratios) and make raw values usable by estimators (such as encoding categories).

### 2. Feature vs. target
Features (`X`) are inputs available to make a prediction. The target (`y`) is the outcome the model is trained to predict. Do not include the target or a proxy that reveals it in the features.

### 3. Total price

```python
df["total_price"] = df["quantity"] * df["unit_price"]
```

Check units and whether discounts, taxes, and currency conversion should be included.

### 4. Profit margin

```python
import numpy as np

df["profit_margin"] = np.where(
    df["revenue"].ne(0),
    df["profit"] / df["revenue"],
    np.nan
)
```

A missing result for zero revenue is preferable to silently creating infinity. The business may instead define a documented special case.

### 5. Age from birth date

```python
today = pd.Timestamp.today().normalize()
birth = pd.to_datetime(df["birth_date"], errors="coerce")
df["age_years"] = ((today - birth).dt.days / 365.2425).astype("float")
```

For a reproducible historical dataset, use a fixed prediction date or each row's prediction timestamp instead of the current date. Birth date must be available and appropriate to use.

### 6. Color
Color is nominal. One-hot encoding is a common choice because there is no natural ranking among red, blue, and green.

### 7. Satisfaction
Low, medium, and high are ordinal. A mapping such as `{"low": 0, "medium": 1, "high": 2}` preserves order, provided the assumed ordering is valid. The numeric gaps should not automatically be interpreted as equal real-world differences.

### 8. Unordered label encoding
Mapping colors to 0, 1, and 2 can imply an artificial ordering or distance to algorithms that use numeric geometry. One-hot encoding avoids that artificial order for nominal features.

### 9. Scaling
Scaling is often useful for linear models with regularization, k-nearest neighbors, SVMs, and gradient-based methods. Tree splits depend on ordering and thresholds, so scaling usually matters less for tree ensembles.

### 10. Imputation
- Mean: simple for roughly symmetric numeric data, but sensitive to outliers.
- Median: robust for skewed numeric data.
- Most frequent: useful for categorical data.
All learned imputation statistics should be fitted on training data only. Missingness indicators may help when absence is informative.

### 11. Outliers
Outliers are observations far from typical values or expected ranges. They may be valid rare events or data errors; deleting them can remove important cases or bias the sample.

### 12. Target leakage
If a model predicts loan default but uses `collections_action_taken_after_default`, it can exploit information unavailable before default. Remove the field or rebuild it using only data available at the prediction time.

## Intermediate: pandas and scikit-learn

### 13. Price per item

```python
df["price_per_item"] = np.where(
    df["quantity"].gt(0),
    df["total_price"] / df["quantity"],
    np.nan
)
```

### 14. Month and weekday

```python
ts = pd.to_datetime(df["timestamp"], errors="coerce")
df["month"] = ts.dt.month
df["day_of_week"] = ts.dt.dayofweek  # Monday=0, Sunday=6
```

### 15. Weekend indicator

```python
ts = pd.to_datetime(df["date"], errors="coerce")
df["is_weekend"] = ts.dt.dayofweek.isin([5, 6]).astype("int8")
```

Timezone and business-specific calendars may matter for real applications.

### 16. Conversion rate

```python
df["conversion_rate"] = np.where(
    df["visits"].gt(0),
    df["purchases"] / df["visits"],
    np.nan
)
```

Confirm the numerator is not greater than the denominator unless the data definition permits it.

### 17. One-hot encoding with pandas

```python
encoded = pd.get_dummies(df, columns=["color"], dtype=int)
```

This is convenient for exploration. In production, separately encoding train and inference data can yield different columns. A fitted `OneHotEncoder(handle_unknown="ignore")` inside a pipeline keeps the learned mapping consistent.

### 18. Median imputation

```python
from sklearn.impute import SimpleImputer

imputer = SimpleImputer(strategy="median")
train_imputed = imputer.fit_transform(X_train_numeric)
test_imputed = imputer.transform(X_test_numeric)
```

Do not call `fit_transform` on the test data.

### 19. Scaling methods

```python
from sklearn.preprocessing import StandardScaler, MinMaxScaler, RobustScaler

values = np.array([[1.0], [2.0], [3.0], [100.0]])
for scaler in [StandardScaler(), MinMaxScaler(), RobustScaler()]:
    print(type(scaler).__name__, scaler.fit_transform(values).ravel())
```

- StandardScaler uses mean and standard deviation and can be affected by extremes.
- MinMaxScaler maps observed minimum and maximum to a range, commonly 0–1; an extreme can compress most values.
- RobustScaler uses median and interquartile range and is less sensitive to extreme values.
Fit each scaler on training data in a real modeling workflow.

### 20. Log transform

```python
df["log_revenue"] = np.log1p(df["revenue"])
```

`log1p` handles zero but not values less than or equal to -1. Confirm the data's domain and consider a suitable alternative for negative values. A transformation should be evaluated rather than assumed to improve performance.

### 21. Binning

```python
df["age_group"] = pd.cut(
    df["age"],
    bins=[0, 18, 35, 60, 120],
    labels=["under_18", "18_34", "35_59", "60_plus"],
    right=False
)
```

Binning can improve interpretability or represent non-linear effects, but it loses within-bin detail and results depend on chosen cut points. Validate the boundaries for the actual domain.

### 22. Polynomial and interaction features

```python
from sklearn.preprocessing import PolynomialFeatures

poly = PolynomialFeatures(degree=2, include_bias=False)
X_poly = poly.fit_transform(X_numeric_train)
print(poly.get_feature_names_out(numeric_feature_names))
```

For evaluation, place the transformer in a pipeline so it is fit within each training fold. Polynomial expansion can increase feature count quickly.

### 23. IQR outlier flags

```python
q1 = df["value"].quantile(0.25)
q3 = df["value"].quantile(0.75)
iqr = q3 - q1
lower = q1 - 1.5 * iqr
upper = q3 + 1.5 * iqr

df["possible_outlier"] = ~df["value"].between(lower, upper)
```

This flags observations for review; it does not prove they are errors. For a predictive pipeline, estimate thresholds from training data only.

### 24–25. Preprocessing pipeline

```python
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.linear_model import LogisticRegression

numeric_features = ["age", "income", "monthly_spend"]
categorical_features = ["region", "plan"]

numeric_pipe = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler()),
])
categorical_pipe = Pipeline([
    ("imputer", SimpleImputer(strategy="most_frequent")),
    ("onehot", OneHotEncoder(handle_unknown="ignore")),
])

preprocess = ColumnTransformer([
    ("num", numeric_pipe, numeric_features),
    ("cat", categorical_pipe, categorical_features),
])

model = Pipeline([
    ("preprocess", preprocess),
    ("classifier", LogisticRegression(max_iter=1000)),
])
model.fit(X_train, y_train)
```

The column names must match the input DataFrame. Keep transformations and estimator together so training and prediction use the same steps.

### 26. Why split first?
Imputers, scalers, encoders, and selectors can learn values or structure from the data. Fitting them before splitting lets test-set information influence training, making evaluation too optimistic. Pipelines ensure each fold learns transformations from its training partition.

### 27. SelectKBest inside a pipeline

```python
from sklearn.feature_selection import SelectKBest, f_classif
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

selection_model = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scale", StandardScaler()),
    ("select", SelectKBest(score_func=f_classif, k=5)),
    ("classifier", LogisticRegression(max_iter=1000)),
])
selection_model.fit(X_train_numeric, y_train)
```

`f_classif` is a simple univariate classifier-oriented choice for numeric features. Other scoring functions may be more suitable for categorical or non-linear relationships. Tune `k` using cross-validation, not the test set.

### 28. Unseen categories
Use an encoder such as `OneHotEncoder(handle_unknown="ignore")`, or define an explicit “other” category where appropriate. Monitor how often new categories occur; frequent unseen values may signal drift or a changed upstream system.

## Advanced: design, validation, and trade-offs

### 29. Average order value
Aggregate only transactions that occurred before the prediction timestamp. For example, calculate historical customer spend divided by historical order count within a defined lookback window. Do not include the current/future order if the model predicts its value or outcome.

### 30. Cyclical hour encoding

```python
hour = df["timestamp"].dt.hour
df["hour_sin"] = np.sin(2 * np.pi * hour / 24)
df["hour_cos"] = np.cos(2 * np.pi * hour / 24)
```

This represents the cyclic relationship between 23:00 and 00:00 as close in feature space. Whether it helps depends on the task.

### 31. High-cardinality postal code
Options include: (1) group codes into broader regions using a documented mapping; this may lose local detail, (2) one-hot encode if cardinality and sample size permit; this can create a wide sparse matrix, or (3) use a carefully validated encoding strategy; target-based encodings can leak labels if not cross-fitted. Evaluate on realistic splits and consider privacy and proxy-discrimination risks.

### 32. Test a new feature
Form a hypothesis, add the feature to a controlled pipeline, and compare with the existing baseline using the same cross-validation folds and predeclared metric. Keep the test set untouched until the approach is finalized. Consider complexity, stability, and operational cost as well as score.

### 33. Time-aware forecasting features
Use rolling counts/means and lag values computed only from earlier timestamps. Split chronologically, ensure rolling windows exclude the current/future target, and evaluate on a later time period. Handle seasonality and changes in data collection.

### 34. Selection vs. dimensionality reduction
Feature selection keeps a subset of original features. Dimensionality reduction transforms features into a smaller set of new representations, such as principal components. Selection often retains clearer business meaning; reduction may compact correlated information but can reduce interpretability.

### 35. Importance vs. causation
Importance describes how a feature contributes to prediction in a specific model/data context. It does not establish cause. Correlated features may share or substitute for importance, and importance can vary across models or samples.

### 36. Training-serving skew
Create unit tests for feature formulas and data types; run the same transformation code for offline and online data; compare feature distributions and missingness; test edge cases; version the pipeline; and log sample inputs/outputs with appropriate privacy controls.

## Mini-project reference outline

```python
# Example outline; adapt column names to your dataset.
X = df.drop(columns=["churn"])
y = df["churn"]

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)

# Create feature columns only from fields available at prediction time.
# Define numeric_features and categorical_features.
# Put imputers, scalers, encoders, feature selection, and estimator
# inside a Pipeline/ColumnTransformer.
# Compare a DummyClassifier baseline with the candidate on the same split.
# Report precision, recall, F1, ROC-AUC, and confusion matrix.
```

Use `DummyClassifier(strategy="most_frequent")` as a simple baseline. Do not assume a new feature helps until you validate it. The project should include feature definitions, limitations, and monitoring.

## Feature specification template

| Field | Description |
|---|---|
| Feature name | Stable, readable identifier |
| Business meaning | What the feature represents |
| Source fields | Raw inputs used |
| Formula | Transformation or aggregation |
| Data type | Numeric, categorical, datetime, text, etc. |
| Missing-value policy | Imputation, flag, or handling rule |
| Prediction-time availability | When the value is known |
| Validation tests | Ranges, null handling, and edge cases |
| Owner/version | Responsible team and version |

## Example monitoring plan

- Monitor schema changes, missingness, category frequencies, ranges, and feature drift.
- Compare offline and online feature calculations.
- Track prediction distributions and downstream metrics when labels become available.
- Investigate changes by relevant population segments.
- Version feature code and data definitions; define retraining or rollback criteria.
