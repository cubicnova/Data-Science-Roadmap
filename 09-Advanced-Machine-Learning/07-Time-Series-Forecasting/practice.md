# Practice Exercises: Time Series Forecasting

Complete these exercises using Python, pandas, NumPy, and scikit-learn. Use chronological splits and explain your reasoning.

## Exercise 1 — Inspect a time series

Create a daily date range of 180 days and a synthetic demand series containing:
- a baseline level of 100,
- a small upward trend,
- a weekly pattern,
- random noise.

1. Store the date and demand in a DataFrame.
2. Parse the date column and sort chronologically.
3. Plot demand over time.
4. Describe the trend and weekly seasonality you can observe.

## Exercise 2 — Naive forecast baseline

Using the series from Exercise 1:
1. Reserve the last 28 days as a test set.
2. Forecast every test point using the last value observed before the test period.
3. Calculate MAE and RMSE.
4. Explain one strength and one weakness of this baseline.

## Exercise 3 — Seasonal-naive forecast

1. Use the last 28 days as the test set and a seasonal period of 7 days.
2. Forecast each test day using the value from seven days earlier, recursively or by repeating the last observed weekly pattern.
3. Calculate MAE and RMSE.
4. Compare the result with Exercise 2. Why might it perform better or worse?

## Exercise 4 — Build lag features

Create a supervised learning table with:
- `lag_1`: previous day's demand,
- `lag_7`: demand seven days earlier,
- `rolling_mean_7`: mean demand over the previous seven days,
- `day_of_week`: weekday number.

Requirements:
1. Ensure `rolling_mean_7` excludes the current day's target.
2. Remove rows with missing lag values.
3. Explain why shifting before calculating the rolling mean matters.

## Exercise 5 — Train a regression model

1. Use lag features from Exercise 4 to train a `RandomForestRegressor`.
2. Use a chronological split with the final 28 valid rows as test data.
3. Fit only on the training period.
4. Compare MAE and RMSE with a naive baseline.
5. State whether the test design matches one-step-ahead predictions or a fixed multi-step forecast, and explain the distinction.

## Exercise 6 — Walk-forward validation

Use `TimeSeriesSplit` with at least 4 splits on the feature table.

1. Calculate MAE for each validation fold.
2. Report the mean and standard deviation of fold MAE.
3. Explain why a random `KFold` split can provide an overly optimistic estimate for forecasting.

## Exercise 7 — Forecast horizon and feature availability

For each feature below, mark it as generally available or unavailable when forecasting tomorrow's demand at the end of today:
- today's demand,
- tomorrow's actual demand,
- tomorrow's known weekday,
- next month's actual average demand,
- a weather forecast issued today.

Explain why data availability is part of leakage prevention.

## Exercise 8 — Metric choice

Suppose actual demand is `[0, 10, 20]` and forecasts are `[3, 8, 25]`.

1. Calculate MAE.
2. Explain why MAPE is problematic for this series.
3. Recommend one additional metric or evaluation view.

## Exercise 9 — Mini-project

Build a reproducible daily demand forecasting experiment.

Deliverables:
- A line plot of the full series.
- A chronological train/test split.
- Naive, seasonal-naive, and lag-feature regression forecasts.
- MAE and RMSE for each model.
- A plot comparing actual values with forecasts.
- A short conclusion on model selection, forecast horizon, and limitations.

## Self-review checklist

- [ ] Dates are sorted and have an understood frequency.
- [ ] Test observations occur after training observations.
- [ ] Features contain only information available at prediction time.
- [ ] Every model is compared on the same test dates.
- [ ] Baselines are included.
- [ ] Metric limitations are documented.
