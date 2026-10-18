# Solutions: Time Series Forecasting

These are reference solutions. Your values may differ if you use a different random seed or data-generating process. The key requirements are chronological evaluation, correctly aligned features, and leakage prevention.

## Exercise 1 — Inspect a time series

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

rng = np.random.default_rng(42)
dates = pd.date_range("2024-01-01", periods=180, freq="D")
t = np.arange(len(dates))
weekly = 8 * np.sin(2 * np.pi * t / 7)
noise = rng.normal(0, 3, len(dates))
demand = 100 + 0.08 * t + weekly + noise

df = pd.DataFrame({"date": dates, "demand": demand})
df["date"] = pd.to_datetime(df["date"])
df = df.sort_values("date").reset_index(drop=True)

df.plot(x="date", y="demand", figsize=(12, 4), title="Synthetic Daily Demand")
plt.ylabel("Demand")
plt.show()
```

The trend term increases by 0.08 units per day. The sinusoidal term adds a repeating weekly pattern, while random noise makes the observations irregular.

## Exercise 2 — Naive forecast baseline

```python
from sklearn.metrics import mean_absolute_error, mean_squared_error
import numpy as np

train = df.iloc[:-28].copy()
test = df.iloc[-28:].copy()

naive_pred = np.repeat(train["demand"].iloc[-1], len(test))
mae = mean_absolute_error(test["demand"], naive_pred)
rmse = np.sqrt(mean_squared_error(test["demand"], naive_pred))
print({"MAE": mae, "RMSE": rmse})
```

Strength: simple, transparent, and difficult to accidentally overcomplicate. Weakness: a flat forecast cannot represent a trend or repeating weekly pattern.

## Exercise 3 — Seasonal-naive forecast

For a 28-day horizon with period 7, repeat the final observed seven-day pattern four times:

```python
season = 7
seasonal_pattern = train["demand"].iloc[-season:].to_numpy()
seasonal_pred = np.tile(seasonal_pattern, int(np.ceil(len(test) / season)))[:len(test)]

seasonal_mae = mean_absolute_error(test["demand"], seasonal_pred)
seasonal_rmse = np.sqrt(mean_squared_error(test["demand"], seasonal_pred))
print({"MAE": seasonal_mae, "RMSE": seasonal_rmse})
```

This may beat the flat baseline when weekly behavior is strong. It may struggle if the trend or seasonal pattern changes significantly.

## Exercise 4 — Build lag features

```python
features = df.copy()
features["lag_1"] = features["demand"].shift(1)
features["lag_7"] = features["demand"].shift(7)
features["rolling_mean_7"] = features["demand"].shift(1).rolling(7).mean()
features["day_of_week"] = features["date"].dt.dayofweek
features = features.dropna().reset_index(drop=True)
```

The shift occurs before the rolling calculation so the rolling window ends at the previous day. Without that shift, the current target would contribute to its own predictor.

## Exercise 5 — Train a regression model

```python
from sklearn.ensemble import RandomForestRegressor

feature_cols = ["lag_1", "lag_7", "rolling_mean_7", "day_of_week"]
train_f = features.iloc[:-28]
test_f = features.iloc[-28:]

model = RandomForestRegressor(
    n_estimators=300, min_samples_leaf=3, random_state=42
)
model.fit(train_f[feature_cols], train_f["demand"])
pred = model.predict(test_f[feature_cols])

print("MAE:", mean_absolute_error(test_f["demand"], pred))
print("RMSE:", np.sqrt(mean_squared_error(test_f["demand"], pred)))
```

This setup uses the observed demand for the previous day as a feature at each test timestamp, so it represents **rolling one-step-ahead predictions** when those observations would be available each day. A fixed 28-day-ahead forecast issued once at the end of training cannot use actual demand from inside the test period; it needs recursive predictions or a direct multi-horizon approach.

## Exercise 6 — Walk-forward validation

```python
from sklearn.model_selection import TimeSeriesSplit
from sklearn.metrics import mean_absolute_error
from sklearn.ensemble import RandomForestRegressor
import numpy as np

X = features[feature_cols]
y = features["demand"]
tscv = TimeSeriesSplit(n_splits=5)
fold_mae = []

for train_idx, val_idx in tscv.split(X):
    fold_model = RandomForestRegressor(
        n_estimators=200, min_samples_leaf=3, random_state=42
    )
    fold_model.fit(X.iloc[train_idx], y.iloc[train_idx])
    fold_pred = fold_model.predict(X.iloc[val_idx])
    fold_mae.append(mean_absolute_error(y.iloc[val_idx], fold_pred))

print("Fold MAE:", fold_mae)
print("Mean MAE:", np.mean(fold_mae))
print("MAE standard deviation:", np.std(fold_mae))
```

Chronological validation mirrors forecasting from past to future. Random K-fold can mix later observations into training folds used to predict earlier observations, which does not match the real prediction setting.

## Exercise 7 — Forecast horizon and feature availability

| Feature | Available at end of today? | Reason |
|---|---|---|
| Today's demand | Generally yes | It has already been observed, assuming reporting is complete |
| Tomorrow's actual demand | No | It is the target being forecast |
| Tomorrow's known weekday | Yes | Calendar information is known in advance |
| Next month's actual average demand | No | It uses future outcomes |
| Weather forecast issued today | Usually yes | Only if the forecast was genuinely available at prediction time |

A feature's value is not enough; its timestamp and publication/availability time matter too.

## Exercise 8 — Metric choice

Actual: `[0, 10, 20]`; forecast: `[3, 8, 25]`.

Absolute errors are `[3, 2, 5]`, so:

\[
MAE = (3 + 2 + 5) / 3 = 3.33
\]

MAPE is problematic because division by the first actual value (zero) is undefined. Consider MAE, RMSE, or a scaled metric such as MASE where an appropriate seasonal or naive scale is available. Always report the scale and baseline used.

## Exercise 9 — Mini-project reference structure

```python
# 1. Create/inspect df with date and demand columns.
# 2. Sort by date and plot.
# 3. Split chronologically; reserve the last 28 days for test.
# 4. Fit baselines and a regression model.
# 5. Evaluate all models on identical dates.
# 6. Plot actual and predictions.
# 7. Document horizon, feature availability, and limitations.
```

Example comparison table:

```python
results = pd.DataFrame([
    {"model": "naive", "MAE": mae, "RMSE": rmse},
    {"model": "seasonal_naive", "MAE": seasonal_mae, "RMSE": seasonal_rmse},
    {"model": "random_forest", "MAE": mean_absolute_error(test_f["demand"], pred),
     "RMSE": np.sqrt(mean_squared_error(test_f["demand"], pred))}
])
print(results.sort_values("MAE"))
```

Do not assume the most complex model is best. Choose based on the validation design, performance against baselines, stability, operational constraints, and forecast horizon.
