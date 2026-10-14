# Solutions — NumPy Practice Exercises

Reference answers for all exercises. Equivalent correct solutions are acceptable.

## 1. Create an array

```python
import numpy as np
np.arange(1, 11)
```

## 2. Use arange

```python
np.arange(2, 21, 2)
```

## 3. Use linspace

```python
np.linspace(0, 1, 6)
```

## 4. Inspect metadata

```python
a = np.arange(12).reshape(3, 4)
print(a.shape, a.ndim, a.size, a.dtype)
```

## 5. Create zeros and ones

```python
print(np.zeros((2, 3)))
print(np.ones((3, 2)))
```

## 6. Create an identity matrix

```python
np.eye(4, dtype=int)
```

## 7. Index one element

```python
np.arange(20).reshape(4, 5)[2, 3]
```

## 8. Select a row

```python
np.arange(12).reshape(3, 4)[1, :]
```

## 9. Select a column

```python
np.arange(12).reshape(3, 4)[:, -1]
```

## 10. Slice a submatrix

```python
np.arange(25).reshape(5, 5)[1:4, 1:4]
```

## 11. Reverse an array

```python
np.array([1, 2, 3, 4, 5])[::-1]
```

## 12. Step slicing

```python
np.arange(20)[::2]
```

## 13. Update selected values

```python
a = np.arange(1, 11)
a[a > 6] = 0
print(a)
```

## 14. Boolean filtering

```python
a = np.arange(1, 31)
a[a % 3 == 0]
```

## 15. Fancy indexing

```python
np.arange(10)[[1, 4, 7]]
```

## 16. Elementwise arithmetic

```python
a = np.array([8, 12, 16]); b = np.array([2, 4, 8])
print(a+b, a-b, a*b, a/b)
```

## 17. Vectorized transformation

```python
np.arange(1, 10).reshape(3, 3) ** 2
```

## 18. Aggregate values

```python
a = np.arange(1, 11)
print(np.sum(a), np.min(a), np.max(a), np.mean(a))
```

## 19. Reduce by axis

```python
m = np.arange(12).reshape(3, 4)
print('row sums:', np.sum(m, axis=1))
print('column sums:', np.sum(m, axis=0))
```

## 20. Broadcast a scalar

```python
np.arange(6).reshape(2, 3) * 10
```

## 21. Broadcast vectors

```python
m = np.array([[1, 2, 3], [4, 5, 6]])
v = np.array([10, 20, 30])
m + v
```

## 22. Normalize values

```python
a = np.array([2., 4., 6., 8.])
a - np.mean(a)
```

## 23. Reshape values

```python
np.arange(24).reshape(2, 3, 4)
```

## 24. Flatten a matrix

```python
np.arange(12).reshape(3, 4).flatten()
```

## 25. Add an axis

```python
np.arange(5)[:, np.newaxis].shape
```

## 26. Concatenate arrays

```python
np.concatenate((np.array([[1, 2], [3, 4]]), np.array([[5, 6], [7, 8]])), axis=0)
```

## 27. Stack arrays

```python
np.stack((np.arange(4), np.arange(4, 8)), axis=0).shape
```

## 28. Split an array

```python
np.split(np.arange(18), 3)
```

## 29. Find statistical summaries

```python
a = np.array([12, 15, 18, 21, 24, 30])
print(np.median(a), np.std(a), np.percentile(a, 75))
```

## 30. Handle NaN

```python
np.nanmean([2.0, 4.0, np.nan, 8.0])
```

## 31. Find values by condition

```python
np.where(np.array([28, 32, 35, 27, 31]) > 30, 1, 0)
```

## 32. Random reproducibility

```python
rng1 = np.random.default_rng(42)
rng2 = np.random.default_rng(42)
a = rng1.integers(0, 100, size=10)
b = rng2.integers(0, 100, size=10)
print(a)
print(np.array_equal(a, b))
```

## 33. Count condition matches

```python
m = np.arange(20).reshape(4, 5)
np.sum(m >= 10)
```

## 34. Mini-project: marks analysis

```python
marks = np.array([[80, 70, 90], [75, 85, 88], [90, 92, 84], [65, 78, 80], [88, 82, 95]])
print('subject means:', np.mean(marks, axis=0))
print('highest per student:', np.max(marks, axis=1))
print('overall median:', np.median(marks))
```

## 35. Mini-project: sales analysis

```python
sales = np.array([[10, 20, 15], [12, 18, 17], [14, 22, 16], [9, 25, 20], [16, 21, 18], [18, 19, 22], [13, 24, 19]])
daily_total = np.sum(sales, axis=1)
print('product totals:', np.sum(sales, axis=0))
print('best day index:', np.argmax(daily_total))
print('lowest day index:', np.argmin(daily_total))
```

