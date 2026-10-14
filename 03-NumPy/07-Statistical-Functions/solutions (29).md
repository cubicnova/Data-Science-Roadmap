# Solutions — NumPy Statistical Functions

Reference answers for the practice exercises. Equivalent correct approaches are acceptable.

## 1. Mean

```python
import numpy as np
a = np.array([10, 20, 30, 40, 50])
np.mean(a)
```

## 2. Median, odd length

```python
np.median([9, 2, 7, 4, 5])
```

## 3. Median, even length

```python
np.median([10, 20, 30, 40])
```

## 4. Mean versus median

```python
a = np.array([10, 12, 13, 15, 100])
print(np.mean(a), np.median(a))  # median is less affected by the outlier
```

## 5. Minimum and maximum

```python
a = np.array([8, 3, 12, 6, 9])
print(np.min(a), np.max(a))
```

## 6. Range

```python
np.ptp([14, 3, 22, 9])
```

## 7. Index of minimum

```python
np.argmin([6, 2, 9, 1, 7])
```

## 8. Index of maximum

```python
np.argmax([6, 2, 9, 1, 7])
```

## 9. Population variance

```python
np.var([2, 4, 6, 8], ddof=0)
```

## 10. Sample variance

```python
np.var([2, 4, 6, 8], ddof=1)
```

## 11. Population standard deviation

```python
np.std([2, 4, 6, 8], ddof=0)
```

## 12. Sample standard deviation

```python
np.std([2, 4, 6, 8], ddof=1)
```

## 13. 25th percentile

```python
np.percentile(np.arange(1, 11), 25)
```

## 14. Quartiles

```python
np.percentile([4, 8, 12, 16, 20], [25, 50, 75])
```

## 15. Quantile

```python
np.quantile(np.arange(1, 11), 0.8)
```

## 16. Column means

```python
m = np.array([[1, 2], [3, 4], [5, 6]])
np.mean(m, axis=0)
```

## 17. Row means

```python
m = np.array([[1, 2], [3, 4], [5, 6]])
np.mean(m, axis=1)
```

## 18. Column sums

```python
np.sum(np.array([[1, 2, 3], [4, 5, 6]]), axis=0)
```

## 19. Keep dimensions

```python
m = np.array([[1, 2], [3, 4], [5, 6]])
x = np.mean(m, axis=0, keepdims=True)
print(x, x.shape)
```

## 20. Weighted average

```python
np.average([70, 90], weights=[1, 3])
```

## 21. Missing value mean

```python
a = np.array([1.0, 2.0, np.nan, 5.0])
print(np.mean(a), np.nanmean(a))
```

## 22. Missing value median

```python
np.nanmedian([1.0, np.nan, 9.0, 3.0])
```

## 23. NaN-aware extrema

```python
a = np.array([np.nan, 4, 2, 10])
print(np.nanmin(a), np.nanmax(a))
```

## 24. NaN-aware standard deviation

```python
np.nanstd([1.0, 2.0, np.nan, 3.0])
```

## 25. Correlation coefficient

```python
np.corrcoef([1, 2, 3], [2, 4, 6])
```

## 26. Negative correlation

```python
np.corrcoef([1, 2, 3], [6, 4, 2])
```

## 27. Covariance

```python
np.cov([1, 2, 3], [2, 4, 6])
```

## 28. Boolean count

```python
np.sum(np.array([2, 5, 8, 11, 14]) > 7)
```

## 29. Outlier effect

```python
a = np.array([20, 21, 22, 23, 200])
print('mean:', np.mean(a), 'median:', np.median(a))
```

## 30. Axis and shape

```python
m = np.zeros((4, 3))
print(np.mean(m, axis=0).shape, np.mean(m, axis=1).shape)
```

## 31. Normalize columns

```python
m = np.array([[1, 10], [2, 20], [3, 30]])
centered = m - np.mean(m, axis=0, keepdims=True)
print(centered)
```

## 32. Percentile of scores

```python
np.percentile([45, 55, 65, 75, 85, 95], 90)
```

## 33. Repeated extrema

```python
print(np.argmin([4, 1, 7, 1, 7]), np.argmax([4, 1, 7, 1, 7]))
```

## 34. Summarize a dataset

```python
a = np.array([12, 15, 15, 18, 20, 25])
print('mean', np.mean(a), 'median', np.median(a), 'std', np.std(a), 'range', np.ptp(a))
```

## 35. Mini challenge: class report

```python
marks = np.array([[80, 70, 90], [75, 85, 88], [90, 92, 84], [65, 78, 80], [88, 82, 95]])
print('mean:', np.mean(marks, axis=0))
print('median:', np.median(marks, axis=0))
print('std:', np.std(marks, axis=0))
```

