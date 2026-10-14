# NumPy Statistical Functions

Learn how to summarize numerical data with NumPy's statistical functions, handle axes, and interpret results responsibly.

## Learning objectives
- Calculate mean, median, minimum, maximum, range, variance, and standard deviation.
- Understand percentiles and quantiles.
- Apply statistics along rows or columns using `axis`.
- Work with missing values using `nan*` functions.
- Understand population versus sample variance and standard deviation.
- Use weighted averages and basic correlation.
- Avoid common interpretation and data-quality mistakes.

## 1. Example dataset

```python
import numpy as np
scores = np.array([72, 85, 90, 68, 95, 80])
```

## 2. Central tendency

- `np.mean(a)`: arithmetic average.
- `np.median(a)`: middle value after sorting.
- `np.average(a, weights=...)`: weighted average.

```python
np.mean(scores)       # 81.666...
np.median(scores)     # 82.5
np.average([80, 90], weights=[1, 3])  # 87.5
```

The mean can be strongly affected by outliers. The median is often more robust for skewed data.

## 3. Minimum, maximum, range and extrema positions

```python
np.min(scores)
np.max(scores)
np.ptp(scores)        # max - min
np.argmin(scores)     # index of minimum
np.argmax(scores)     # index of maximum
```

`argmin` and `argmax` return indices, not the values themselves. If the minimum or maximum appears multiple times, NumPy returns the first occurrence.

## 4. Variance and standard deviation

Variance measures average squared deviation from the mean. Standard deviation is its square root and is expressed in the original units.

```python
np.var(scores)
np.std(scores)
```

By default, NumPy uses `ddof=0` (population variance/std). For sample estimates, use `ddof=1`:

```python
np.var(scores, ddof=1)
np.std(scores, ddof=1)
```

For a dataset of size \(N\), the divisor is \(N-\text{ddof}\). Use `ddof=1` when estimating population variability from a sample under the usual sample-variance convention.

## 5. Percentiles and quantiles

```python
np.percentile(scores, 25)
np.percentile(scores, 50)
np.percentile(scores, 75)
np.quantile(scores, 0.25)
```

Percentiles use a 0–100 scale; quantiles use a 0–1 scale. The default interpolation/method may return a value between observed data points.

## 6. Statistics across axes

For a 2-D array, `axis=0` reduces rows and produces one result per column; `axis=1` reduces columns and produces one result per row.

```python
data = np.array([[10, 20, 30], [40, 50, 60]])
np.mean(data, axis=0)  # [25., 35., 45.]
np.mean(data, axis=1)  # [20., 50.]
```

Use `keepdims=True` to retain reduced axes with length 1, which can help with broadcasting:

```python
column_means = np.mean(data, axis=0, keepdims=True)
centered = data - column_means
```

## 7. Missing values

Regular reductions generally propagate `np.nan`:

```python
a = np.array([1.0, 2.0, np.nan, 4.0])
np.mean(a)       # nan
np.nanmean(a)    # 2.333...
np.nanmedian(a)  # 2.0
np.nanstd(a)     # ignores NaN values
```

Related functions include `np.nanmin`, `np.nanmax`, `np.nanvar`, and `np.nanpercentile`. These ignore NaNs, not every kind of invalid or infinite value. All-NaN slices may produce warnings and NaN results.

## 8. Correlation and covariance

- `np.corrcoef(x, y)` returns a correlation matrix, with Pearson correlation coefficients.
- `np.cov(x, y)` returns a covariance matrix (sample covariance by default, with `ddof=1` when normalized by `N-1`).

Correlation is dimensionless and usually ranges from -1 to +1. It describes linear association, not causation. Covariance depends on the variables' units.

## 9. Common mistakes and best practices

1. Distinguish `mean` from `median`; report the statistic that fits the distribution and question.
2. State whether variance/std is population (`ddof=0`) or sample (`ddof=1`).
3. Use the correct `axis`; verify the shape of each result.
4. Handle missing values intentionally instead of silently dropping or filling them.
5. Remember `argmin`/`argmax` return indices.
6. Do not interpret correlation as proof of causation.
7. Inspect outliers and data quality before drawing conclusions.
8. For empty arrays or all-NaN slices, check warnings and returned values.

## Quick reference

| Purpose | NumPy function |
|---|---|
| Mean / median | `mean`, `median`, `average` |
| Minimum / maximum / range | `min`, `max`, `ptp` |
| Indices of extrema | `argmin`, `argmax` |
| Variance / standard deviation | `var`, `std` |
| Percentiles / quantiles | `percentile`, `quantile` |
| Ignore NaNs | `nanmean`, `nanmedian`, `nanstd`, `nanvar` |
| Correlation / covariance | `corrcoef`, `cov` |

## Further reading
- [NumPy statistics routines](https://numpy.org/doc/stable/reference/routines.statistics.html)
- [NumPy `mean`](https://numpy.org/doc/stable/reference/generated/numpy.mean.html)
- [NumPy `std`](https://numpy.org/doc/stable/reference/generated/numpy.std.html)
- [NumPy `percentile`](https://numpy.org/doc/stable/reference/generated/numpy.percentile.html)
