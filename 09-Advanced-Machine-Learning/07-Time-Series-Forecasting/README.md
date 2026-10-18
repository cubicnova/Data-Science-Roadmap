# 07 - Time Series Forecasting

Learn how to understand time-dependent data, build forecasting baselines, evaluate forecasts without data leakage, and compare practical forecasting methods with Python.

## Learning objectives

By the end of this module, you should be able to:

- Recognize trend, seasonality, cycles, noise, and time-series structure.
- Parse dates, set a time index, and visualize observations over time.
- Create naive and seasonal-naive forecasts as reliable baselines.
- Build lag and rolling-window features for supervised forecasting.
- Use chronological train/test splits and walk-forward validation.
- Evaluate predictions with MAE, RMSE, and MAPE while understanding their limitations.
- Train a machine-learning model for a forecasting task.
- Explain forecast limitations and communicate uncertainty responsibly.

## Folder contents

| File | Purpose |
|---|---|
| `README.md` | Concepts, workflow, and setup |
| `examples.ipynb` | Guided, runnable Python examples |
| `practice.md` | Exercises to complete independently |
| `solution.md` | Worked solutions to the practice exercises |

## Prerequisites

- Python fundamentals
- Basic pandas, NumPy, and Matplotlib
- Basic regression and model evaluation

## Setup

Use Python 3.10+ if possible. Install the main dependencies:

```bash
python -m pip install pandas numpy matplotlib scikit-learn jupyter
```

Optional libraries for dedicated time-series models:

```bash
python -m pip install statsmodels
```

Start Jupyter from this folder:

```bash
jupyter notebook
```

Open `examples.ipynb` and run the cells from top to bottom.

## Core concepts

### What is a time series?

A time series is a sequence of observations indexed in time, such as daily sales, hourly electricity demand, monthly revenue, or weekly website traffic. Observations often have dependencies: recent values can contain information about future values.

### Common components

- **Trend:** a long-term increase or decrease.
- **Seasonality:** a pattern that repeats at a known interval, such as weekday demand or annual sales.
- **Cycle:** a longer-term fluctuation without a fixed, known period.
- **Noise:** irregular variation that a model cannot reliably predict.
- **Level:** the typical magnitude around which the series varies.

### Forecasting workflow

1. Define the target, frequency, forecast horizon, and business use.
2. Parse timestamps, sort observations, and inspect missing or duplicate time points.
3. Plot the series and investigate trend, seasonality, outliers, and structural changes.
4. Establish a simple baseline before using complex models.
5. Split chronologically. Never randomly shuffle time-ordered observations for a standard forecasting evaluation.
6. Build features using only information available at the forecast origin.
7. Tune and compare models with rolling-origin or walk-forward validation.
8. Evaluate on a final holdout period using metrics that match the use case.
9. Communicate uncertainty and monitor performance after deployment.

## Forecasting methods covered

| Method | Best used as | Important limitation |
|---|---|---|
| Last-value naive | Quick baseline for persistent series | Cannot model changing trend or seasonality |
| Seasonal naive | Baseline when a pattern repeats | Needs enough history for a full season |
| Moving average | Smoothing short-term noise | Can lag behind rapid changes |
| Lag features + regression | Flexible supervised baseline | Requires careful feature timing and validation |
| Exponential smoothing / ARIMA | Classical statistical approaches | Require model diagnostics and appropriate assumptions |

## Evaluation metrics

For actual values \(y_t\) and predictions \(\hat y_t\):

- **MAE:** average absolute error; easy to interpret in target units.
- **RMSE:** square root of average squared error; penalizes large errors more strongly.
- **MAPE:** average absolute percentage error; can be undefined or unstable when actual values are zero or near zero.

Always compare a candidate model against a meaningful baseline. A metric by itself does not establish that a forecast is useful.

## Time-series leakage checklist

- Do not use random train/test splitting for a standard future forecast.
- Do not compute rolling features using future observations.
- Shift target-derived rolling statistics so they only include prior observations.
- Fit preprocessing and model parameters on training data only.
- Use the correct forecast horizon when constructing labels.
- Do not use data published after the forecast would have been made.
- Keep the final test period untouched during model selection.

## Suggested learning path

1. Run all examples in `examples.ipynb`.
2. Complete `practice.md` without looking at `solution.md`.
3. Compare your answers with `solution.md`.
4. Extend the mini-project with a different seasonal period or a second model.

## Mini-project: Daily demand forecasting

Use a daily demand series to forecast the next 14 days. Compare:
- Last-value naive forecast
- Seasonal-naive forecast (period 7)
- A regression model using lag and calendar features

Use the same test period for every model. Report MAE and RMSE, plot actual versus forecast values, and explain when each model could fail.

## Further reading

- [scikit-learn: Time-related feature engineering](https://scikit-learn.org/stable/auto_examples/applications/plot_time_series_lagged_features.html)
- [scikit-learn: TimeSeriesSplit](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.TimeSeriesSplit.html)
- [pandas: Time series / date functionality](https://pandas.pydata.org/docs/user_guide/timeseries.html)
- [statsmodels: Time series analysis](https://www.statsmodels.org/stable/tsa.html)

## Completion checklist

- [ ] I can plot and describe the main time-series patterns.
- [ ] I can build naive and seasonal-naive baselines.
- [ ] I can create leakage-safe lag and rolling features.
- [ ] I can split and validate data chronologically.
- [ ] I can compare forecasts using appropriate metrics.
- [ ] I can explain forecast uncertainty and limitations.
