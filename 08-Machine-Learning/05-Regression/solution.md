# Reference Solutions — Regression

These are example solutions. Results vary slightly by software version and random split. Adapt methods to the target, data collection process, and cost of errors.

## Beginner: concepts and metrics

### 1. Regression
Regression predicts a numerical quantity, such as house price, delivery time, or monthly electricity use.

### 2. Regression vs. classification
Regression predicts a continuous numeric value. Classification predicts a category or class label, even if the classes are represented by numbers such as 0 and 1.

### 3. House-price target
The target (`y`) is the sale price. Candidate features (`X`) could include floor area, age, location, and number of rooms, provided they are available at prediction time.

### 4. Simple linear regression
The model is \(\hat{y}=b_0+b_1x\). The intercept \(b_0\) is the predicted value when \(x=0\); the slope \(b_1\) is the model's predicted change in \(y\) for a one-unit increase in \(x\). The intercept may not have a practical interpretation if zero is outside the data range.

### 5. Residual
A residual is `observed - predicted`. Positive residuals mean the model underpredicted that observation; negative residuals mean it overpredicted.

### 6. MAE
An MAE of ₹8,000 means the model's predictions differ from observed values by an average absolute amount of ₹8,000 on the evaluated examples. Individual errors can be larger or smaller.

### 7. MAE, MSE, RMSE
- MAE averages absolute errors and is relatively easy to explain.
- MSE averages squared errors and penalizes large errors more strongly.
- RMSE is the square root of MSE and is in the target's units.
MAE and RMSE are both in target units; MAE is often the most straightforward to communicate.

### 8. R²
R² compares the model's squared error with a mean-prediction reference. It is often between 0 and 1 on useful models, but can be negative on held-out data when predictions are worse than the reference under the metric definition.

### 9. Baseline
A baseline checks whether a model adds value beyond a simple rule, such as predicting the training target mean. Evaluate the baseline and candidate on the same test observations.

### 10. Prediction vs. causation
Prediction estimates an outcome from available information. Causal inference asks what would happen under an intervention. Predictive associations alone do not establish causal effects.

### 11. Initial checks
Inspect shape, data types, missingness, duplicates, target range/distribution, feature ranges, and possible target leakage.

### 12. Poor generalization
Possible reasons include overfitting, data leakage that inflated earlier results, distribution shift, poor-quality labels, irrelevant features, or a split that does not reflect deployment.

## Intermediate: model building and diagnostics

### 13–15. Generate, fit, and evaluate

```python
import numpy as np
from sklearn.datasets import make_regression
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score

X, y = make_regression(
    n_samples=1000, n_features=5, n_informative=4,
    noise=20.0, random_state=42
)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.20, random_state=42
)

model = LinearRegression()
model.fit(X_train, y_train)
pred = model.predict(X_test)

mae = mean_absolute_error(y_test, pred)
mse = mean_squared_error(y_test, pred)
rmse = np.sqrt(mse)
r2 = r2_score(y_test, pred)

print({"MAE": mae, "MSE": mse, "RMSE": rmse, "R2": r2})
```

### 16. Mean baseline

```python
from sklearn.dummy import DummyRegressor
from sklearn.metrics import mean_absolute_error

baseline = DummyRegressor(strategy="mean")
baseline.fit(X_train, y_train)
baseline_pred = baseline.predict(X_test)

print("Baseline MAE:", mean_absolute_error(y_test, baseline_pred))
print("Linear MAE:", mean_absolute_error(y_test, pred))
```

The baseline's mean must be learned from training targets only.

### 17. Observed vs. predicted plot

```python
import matplotlib.pyplot as plt

plt.scatter(y_test, pred, alpha=0.6)
low = min(np.min(y_test), np.min(pred))
high = max(np.max(y_test), np.max(pred))
plt.plot([low, high], [low, high], linestyle="--")
plt.xlabel("Observed")
plt.ylabel("Predicted")
plt.title("Observed vs. predicted")
plt.show()
```

Points close to the diagonal have predictions close to observations. Curvature, changing spread, or isolated points far from the line can signal model limitations.

### 18. Residual plot

```python
residuals = y_test - pred
plt.scatter(pred, residuals, alpha=0.6)
plt.axhline(0, linestyle="--")
plt.xlabel("Predicted")
plt.ylabel("Residual (observed - predicted)")
plt.title("Residuals vs. predicted")
plt.show()
```

Concerning patterns include a curve (possible non-linearity) and a funnel shape (non-constant error variance). Clusters or isolated large residuals also deserve investigation.

### 19–20. Ridge, Lasso, and scaling

```python
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import Ridge, Lasso

ridge = make_pipeline(StandardScaler(), Ridge(alpha=1.0))
lasso = make_pipeline(StandardScaler(), Lasso(alpha=0.1, max_iter=10000))
ridge.fit(X_train, y_train)
lasso.fit(X_train, y_train)
```

Ridge penalizes squared coefficient magnitudes; Lasso penalizes absolute coefficient magnitudes and can shrink some coefficients to zero. Scaling prevents features measured on large numerical scales from dominating regularization solely due to units. Tune `alpha` using cross-validation.

### 21. Polynomial regression

```python
from sklearn.preprocessing import PolynomialFeatures
from sklearn.pipeline import make_pipeline
from sklearn.linear_model import LinearRegression

models = {
    degree: make_pipeline(
        PolynomialFeatures(degree=degree, include_bias=False),
        LinearRegression()
    )
    for degree in [1, 2, 5]
}
```

Fit each model using the training split and compare validation/CV errors. Higher degrees can represent more complex curves but may fit noise. Use a pipeline so polynomial terms are constructed consistently.

### 22. Overfitting
A model is overfitting when it captures training-specific noise and generalizes poorly. A very low training error paired with much higher validation/test error is a warning sign.

### 23. Multicollinearity
Highly correlated predictors can make individual coefficients unstable or difficult to interpret. Predictions may still be acceptable. Inspect correlations and domain meaning, consider regularization, and avoid causal claims based solely on coefficient size.

### 24. Chronological split
Use a time-based split when predicting future events from past observations. Random splitting can leak future patterns into training and create an unrealistic evaluation.

### 25–26. Leakage-safe mixed-data pipeline

```python
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.linear_model import Ridge

numeric_features = ["area", "age", "rooms"]
categorical_features = ["city", "property_type"]

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
    ("regressor", Ridge(alpha=1.0)),
])
model.fit(X_train_df, y_train)
```

Use the actual column names in your DataFrame. Imputers, scalers, encoders, and feature selectors learn from data; fitting them before splitting lets held-out information influence training.

### 27. Cross-validation comparison

```python
from sklearn.model_selection import KFold, cross_validate
from sklearn.linear_model import LinearRegression, Ridge
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler

cv = KFold(n_splits=5, shuffle=True, random_state=42)
candidates = {
    "LinearRegression": make_pipeline(StandardScaler(), LinearRegression()),
    "Ridge": make_pipeline(StandardScaler(), Ridge(alpha=1.0)),
}
for name, estimator in candidates.items():
    scores = cross_validate(
        estimator, X_train, y_train, cv=cv,
        scoring=["neg_mean_absolute_error", "neg_root_mean_squared_error", "r2"]
    )
    print(name, {
        key: float(scores[key].mean())
        for key in scores if key.startswith("test_")
    })
```

Scikit-learn represents error metrics as negative scores in cross-validation so that larger scores are always better. Multiply negative MAE/RMSE values by -1 to report positive errors.

### 28. MAE = 12.5 minutes
Predictions differ from observed delivery durations by an average absolute amount of 12.5 minutes on the evaluated examples. It does not mean every prediction is within 12.5 minutes.

### 29. MAE = 4 and RMSE = 15
A much larger RMSE than MAE suggests some errors may be much larger than typical errors, because RMSE penalizes large errors more strongly. Inspect residuals and influential cases.

### 30. Subgroup reliability
A strong overall score can hide poor performance in a small region, price band, or population group. Report segment-level errors when appropriate and ensure the evaluation sample supports those comparisons.

## Advanced: model evaluation and trade-offs

### 31. Heteroscedastic data

```python
rng = np.random.default_rng(42)
x = np.linspace(0, 10, 500)
noise = rng.normal(0, 0.5 + 0.4 * x, size=len(x))
y = 2 + 3 * x + noise

plt.scatter(x, y, alpha=0.5)
plt.xlabel("x")
plt.ylabel("y")
plt.title("Synthetic data with increasing error variance")
plt.show()
```

The spread increases with `x`. A residual plot may reveal a funnel shape. Consider whether prediction intervals, transformations, weighted methods, or a different model are appropriate for the use case.

### 32. Fair Ridge/Lasso comparison
Use the same folds, target, eligible features, preprocessing strategy, and scoring metric. Tune regularization within training cross-validation and do not choose hyperparameters from test results.

### 33. Skewed positive target
A log transformation can reduce right skew and make multiplicative relationships easier to model. However, predictions transformed back to the original scale can be biased, and zeros/negative values require care. Evaluate on the business-relevant original scale where possible.

### 34. Outliers
Least squares squares residuals, so large errors can have disproportionate influence. Inspect data quality, leverage, and residuals. Depending on the task, consider robust regression such as `HuberRegressor` or `RANSACRegressor`, but do not remove valid extreme observations without justification.

### 35. Next-month sales
Use earlier months for training and a later time period for validation/test. Construct lag and rolling features using past values only, evaluate with MAE/RMSE and business-specific metrics, and consider seasonal baselines. Do not randomly mix future observations into training.

### 36. Coefficients and causation
Coefficients reflect fitted associations conditional on the included variables and modeling assumptions. Confounding, selection bias, measurement errors, and correlated predictors can distort interpretation.

### 37. Regression leakage examples
- Using the final sale price or a post-sale adjustment to predict sale price.
- Computing an imputation/scaling statistic on the full dataset before splitting.
- Creating a rolling feature that includes future target values.
- Duplicated entities appearing in both training and test when deployment is to unseen entities.

### 38. Delayed-label monitoring
Track input schema, missingness, feature drift, prediction distribution, latency, and failure rates immediately. When labels arrive, calculate MAE/RMSE and segment-level errors, compare with a baseline, and define thresholds for investigation or retraining.

## Mini-project reference outline

```python
from sklearn.model_selection import train_test_split
from sklearn.dummy import DummyRegressor
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score
import numpy as np

# X contains eligible features; y is the continuous target.
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

baseline = DummyRegressor(strategy="mean")
baseline.fit(X_train, y_train)
baseline_pred = baseline.predict(X_test)

# Fit your leakage-safe preprocessing + regression Pipeline here.
# Evaluate the baseline and final candidate on the same held-out rows.
def report_regression(y_true, y_pred):
    return {
        "MAE": mean_absolute_error(y_true, y_pred),
        "RMSE": np.sqrt(mean_squared_error(y_true, y_pred)),
        "R2": r2_score(y_true, y_pred),
    }
```

For housing data, check whether property identifiers, final sale information, or post-sale fields leak the target. A random split may be unsuitable if the real goal is future prices or generalization to unseen locations.

## Example model-card template

- **Model name/version:**
- **Intended use and prediction time:**
- **Target and units:**
- **Dataset/source and time range:**
- **Split strategy:**
- **Baseline metrics:**
- **Final metrics (MAE, RMSE, R²):**
- **Residual and subgroup findings:**
- **Known limitations:**
- **Monitoring and retraining triggers:**
