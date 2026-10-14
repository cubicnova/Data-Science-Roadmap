# Descriptive Statistics — Reference Solutions

```python
import numpy as np
import pandas as pd

# 1. (4 + 6 + 8 + 10) / 4 = 7
# 2. Median = 9
# 3. Median = (4 + 8) / 2 = 6
# 4. Mode = 4
# 5. Range = 21 - 5 = 16
# 6. An extreme value can pull the mean toward itself; the median is based
#    on the middle position and is usually less sensitive to extreme values.
# 7. Values [2,4,6]: population mean 4; variance = 8/3; std = sqrt(8/3).
# 8. Sample variance = [(2-4)^2 + (4-4)^2 + (6-4)^2] / (3-1) = 4.
# 9. Population variance divides by N; sample variance commonly divides by n-1.
# 10. Example [1,2,3,4,5,6,7,8]: quartile values depend on convention.
#     Under the median-of-halves convention Q1=2.5, median=4.5, Q3=6.5, IQR=4.
# 11. Potential outliers are values below Q1-1.5*IQR or above Q3+1.5*IQR.
# 12. IQR = 18-10 = 8; fences: 10-12=-2 and 18+12=30.
# 13. Relative frequency = 18/60 = 0.3 = 30%.
# 14.
labels = pd.Series(["red", "blue", "red", "green", "blue", "red"])
print(labels.value_counts())

# 15. Histogram groups numeric values into bins; bar chart compares categories.
# 16. Right skew has a longer upper tail; mean often exceeds median.
# 17. -0.85 suggests strong negative linear association; it does not prove causation.
# 18. Mean = 6; median = 3.
values = [2, 3, 3, 4, 18]
s = pd.Series(values)
print(s.mean(), s.median())

# 19.
print(s.describe())

# 20. Pandas defaults to sample variance/std (ddof=1).
print(s.var(), s.std())

# 21. NumPy defaults to population variance/std (ddof=0).
arr = np.array([2, 4, 6])
print(np.var(arr), np.std(arr))

# 22.
df = pd.DataFrame({"height": [160, 170, 180], "weight": [55, 70, 80]})
print(df.mean(numeric_only=True))

# 23.
df_with_missing = pd.DataFrame({"a": [1, None, 3], "b": [None, "x", "y"]})
print(df_with_missing.isna().sum())

# 24. `labels.value_counts(normalize=True)` returns category proportions.
print(labels.value_counts(normalize=True))

# 25.
print(s.quantile([0.25, 0.50, 0.75]))

# 26.
q1, q3 = s.quantile([0.25, 0.75])
iqr = q3 - q1
lower_fence = q1 - 1.5 * iqr
upper_fence = q3 + 1.5 * iqr
outliers = s[(s < lower_fence) | (s > upper_fence)]
print("Fences:", lower_fence, upper_fence, "Potential outliers:", outliers.tolist())

# 27. An extreme value may be valid and meaningful; check source, context, and rules.
# 28.
equal_mean_median = [1, 2, 3, 4, 5]  # both are 3
# 29.
right_skewed = [1, 1, 2, 2, 3, 10, 30]
# 30. Sample size indicates how much data underlies the summary and supports interpretation.
# 31.
proportions = labels.value_counts(normalize=True)
print(proportions, proportions.sum())  # sums to 1
# 32. count=nonmissing observations; mean=average; std=sample standard deviation;
#     min/max=smallest/largest values.
# 33. Correlation summarizes linear association; nonlinear patterns may have r near zero.

# 34.
def summarize(values):
    series = pd.Series(values, dtype="float64").dropna()
    if series.empty:
        raise ValueError("At least one numeric value is required.")
    return {
        "mean": series.mean(),
        "median": series.median(),
        "range": series.max() - series.min(),
        "sample_std": series.std() if len(series) > 1 else float("nan"),
    }

# 35.
data = pd.Series([10, 11, 12, 12, 13, 100])
print({
    "mean": data.mean(),
    "median": data.median(),
    "std": data.std(),
    "IQR": data.quantile(0.75) - data.quantile(0.25),
})
```

## Challenge example

```python
import pandas as pd

df = pd.DataFrame({"value": [10, 11, 12, 12, 13, 100]})
s = df["value"]
print("Mean:", s.mean())
print("Median:", s.median())
print("Sample standard deviation:", s.std())
print("IQR:", s.quantile(0.75) - s.quantile(0.25))
print(s.describe())
```

The value 100 raises the mean and standard deviation. The median and IQR are less affected, so they may better describe the typical value and middle spread for this example. Always investigate extreme values rather than deleting them automatically.
