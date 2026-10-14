# NumPy Introduction — Reference Solutions

These are example solutions. Other correct approaches are acceptable.

## A. Fundamentals

1. NumPy is a Python library for efficient numerical computing. `ndarray` is its N-dimensional array object.
2. Vectorized operations, multidimensional arrays, and numerical/statistical functions; arrays can also use compact homogeneous dtypes.
3. `python -m pip install numpy`
4. `import numpy as np` then `print(np.__version__)`
5. `np.array([5, 10, 15, 20, 25])`
6. `np.zeros((2, 3))`
7. `np.ones((3, 2))`
8. `np.arange(0, 20, 2)`
9. `np.linspace(0, 1, 7)`
10. `np.eye(4)`

## B. Attributes, types, and arithmetic

11.
```python
a = np.array([[1, 2, 3], [4, 5, 6]])
print(a.ndim, a.shape, a.size, a.dtype)
# 2 (2, 3) 6 integer dtype (exact dtype depends on platform)
```
12. `np.array([1.25, 2.5, 3.75], dtype=np.float32)`
13. `np.array([1.2, 2.8, -1.9]).astype(int)` gives `[1, 2, -1]` on common integer dtypes because conversion truncates toward zero.
14. `a + b -> [3, 7, 11]`; `a - b -> [1, 1, 1]`; `a * b -> [2, 12, 30]`; `a ** 2 -> [4, 16, 36]`.
15. `a + 10`
16. For `a = np.arange(1, 10).reshape(3, 3)`: sum `45`, mean `5.0`, min `1`, max `9`.
17. For `a = np.array([[1, 2, 3], [4, 5, 6]])`: `a.sum(axis=0)` is `[5, 7, 9]`; `a.sum(axis=1)` is `[6, 15]`.
18. `np.std(x, ddof=0)` gives population standard deviation; `np.std(x, ddof=1)` gives sample standard deviation. For `[2,4,6,8]`, these are approximately `2.2361` and `2.5820`.

## C. Indexing, slicing, and shape

19. `a[0]` and `a[-1]`
20. `a[2:6]`
21. `matrix[2, 1]`
22. `matrix[:, 0]` and `matrix[-1, :]`
23. `np.arange(12).reshape(3, 4)`
24. `a.reshape(-1, 1)`
25. `a.ravel()` may return a view if possible; `a.flatten()` always returns a copy.
26.
```python
a = np.arange(6)
view = a[1:4]
view[0] = 99
print(a)  # original array changes
b = np.arange(6)
copy = b[1:4].copy()
copy[0] = 99
print(b)  # b remains unchanged
```

## D. Filtering and broadcasting

27. `a[a > 10]`
28. `a[(a >= 5) & (a <= 15)]`
29. Parentheses ensure each comparison is evaluated before the elementwise `&` operator combines the Boolean arrays.
30.
```python
a = np.array([[1], [2], [3]])  # shape (3, 1)
b = np.array([[10, 20, 30, 40]])  # shape (1, 4)
c = a + b
print(c.shape)  # (3, 4)
```
31.
```python
span = x.max() - x.min()
normalized = (x - x.min()) / span if span != 0 else np.zeros_like(x, dtype=float)
```
When all values are equal, the denominator is zero; define a suitable policy instead of dividing by zero.

## E. Random data and mini tasks

32.
```python
rng = np.random.default_rng(42)
print(rng.integers(0, 10, size=5))
```
33.
```python
rng = np.random.default_rng(42)
samples = rng.normal(10, 2, size=1000)
print(samples.mean())
print(samples.std(ddof=1))
```
Sample results vary with NumPy version or generator implementation; the mean should be near 10 and sample standard deviation near 2.
34.
```python
scores = np.array([55, 72, 89, 67, 95, 70])
print(scores.mean())
print(np.count_nonzero(scores >= 70))
print(scores[scores >= 70])
```
35.
```python
data = np.array([[18, 70], [21, 82], [19, 65], [25, 91]])
print("shape:", data.shape, "dtype:", data.dtype)
print("rows with score >= 70:", data[data[:, 1] >= 70])
print("column means:", data.mean(axis=0))
```
