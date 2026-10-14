# NumPy Arrays and Data Types — Reference Solutions

Use `import numpy as np` for the examples below. Platform-specific inferred dtypes may vary; explicit dtype examples are reproducible.

## A. Arrays and attributes

1. An `ndarray` is NumPy's multidimensional array object.
2. `a = np.array(12); print(a.shape, a.ndim)` gives `() 0`.
3. `np.arange(1, 9)`
4. `np.arange(12).reshape(3, 4)`
5. `np.zeros((2, 3, 4))`
6. For `a = np.zeros((3, 4))`, output is shape `(3, 4)`, ndim `2`, size `12`.
7. Size is the product of shape dimensions; for shape `(3, 4)`, size is `3 * 4 = 12`.
8. `a = np.array([1, 2, 3], dtype=np.int16); print(a.dtype, a.itemsize)` gives `int16` and `2`.
9. `a.strides` reports the number of bytes to step along each axis in the current array layout.
10. `np.array(12).item()` returns Python integer `12`.

## B. Data types and ranges

11.
```python
signed = np.array([-2, 0, 2], dtype=np.int8)
unsigned = np.array([0, 128, 255], dtype=np.uint8)
single = np.array([1.5], dtype=np.float32)
double = np.array([1.5], dtype=np.float64)
flags = np.array([True, False], dtype=np.bool_)
```
12. `info = np.iinfo(np.int8); print(info.min, info.max)` outputs `-128 127`.
13. `np.iinfo(np.uint8)` reports a range from 0 to 255.
14. `np.finfo(np.float32)` and `np.finfo(np.float64)` report precision and range information; `float64` generally offers more precision but uses twice the element storage.
15. Signed integers are needed for negative values; unsigned integers are suitable only when values are non-negative and their maximum fits the dtype.
16. `np.array([1, 2.5, 3])` normally infers a floating-point dtype so all elements can be represented numerically.
17. Fixed-width strings can be padded or truncated to the selected dtype's width; inspect the dtype and avoid choosing too-small widths.
18. `a = np.array([1, 0, 3]); mask = a > 0`

## C. Type conversion and precision

19. `np.array([1.2, 2.8, -3.7]).astype(int)` gives `[1, 2, -3]` on common platforms because truncation is toward zero.
20. `np.rint(values).astype(int)` rounds to nearest integer first; exact half cases follow NumPy's rounding-to-even behavior.
21. Narrowing integer casts can wrap or otherwise produce values that do not represent the original input, depending on dtype and NumPy version. Validate bounds before casting; do not use a cast to preserve an out-of-range value.
22. `np.array([1, 2, 3]).astype(np.float32)`
23. `reshape()` changes the array's shape while preserving element values; `astype()` converts the dtype and normally allocates a new array.
24. `np.asarray()` is useful when an array representation is needed and copying should be avoided where possible; `np.array()` is a general constructor with copying behavior controlled by its arguments and NumPy version.
25.
```python
a = np.array([1.2, 2.8])
b = a.astype(np.int64)
print(np.shares_memory(a, b))  # False
```

## D. Missing values and memory

26. `x = np.array([1.0, np.nan, np.inf])`
27. `np.isnan(x)` gives `[False, True, False]`.
28. `np.isfinite(x)` gives `[True, False, False]`.
29. NaN is not equal to itself, so `x == np.nan` does not reliably detect NaNs. Use `np.isnan(x)`.
30.
```python
a = np.zeros(100, dtype=np.float32)
b = np.zeros(100, dtype=np.float64)
print(a.nbytes, b.nbytes)  # 400 800
```
`float64` uses 8 bytes per element; `float32` uses 4 bytes per element.
31.
```python
a = np.arange(5)
view = a[1:3]
view[0] = 99
print(a)  # original changes
b = np.arange(5)
copy = b[1:3].copy()
copy[0] = 99
print(b)  # original remains [0, 1, 2, 3, 4]
```

## E. Applied tasks

32.
```python
pixels = np.array([0, 64, 128, 255], dtype=np.uint8)
```
Check that every input value is within 0–255 before casting.
33. `a = np.arange(12, dtype=np.float32).reshape(3, 4)`; its dtype remains `float32`.
34.
```python
def all_finite(values):
    arr = np.asarray(values)
    return bool(np.isfinite(arr).all())
```
35. A good report compares `np.iinfo` or `np.finfo` limits, `itemsize`/`nbytes`, expected precision, and whether the dataset's minimum and maximum are representable. For example, `float32` halves memory compared with `float64`, but has less precision; the choice depends on required numerical accuracy.
