# NumPy Array Operations

## 1. Overview

NumPy array operations let you transform and analyze numerical data efficiently. This module covers element-wise arithmetic, comparisons, universal functions (ufuncs), aggregation, broadcasting, sorting, stacking, splitting, and matrix operations.

Start with:

```python
import numpy as np
```

## 2. Element-wise arithmetic

For compatible arrays, arithmetic operates element by element.

```python
a = np.array([1, 2, 3])
b = np.array([10, 20, 30])
a + b   # [11, 22, 33]
a - b   # [-9, -18, -27]
a * b   # [10, 40, 90]
a / b   # [0.1, 0.1, 0.1]
a ** 2  # [1, 4, 9]
```

The `*` operator is element-wise multiplication. For matrix multiplication, use `@` or `np.matmul()`.

## 3. Universal functions (ufuncs)

Ufuncs perform element-wise operations efficiently and usually support broadcasting.

```python
x = np.array([0.0, np.pi / 2, np.pi])
np.sin(x)
np.sqrt(np.array([1.0, 4.0, 9.0]))
np.exp(np.array([0.0, 1.0]))
np.log(np.array([1.0, np.e]))
np.abs(np.array([-2, 0, 3]))
```

For real-valued arrays, `np.log` is the natural logarithm. Zero produces negative infinity, and negative values produce `NaN` with a warning under standard floating-point error handling.

## 4. Comparisons and logical operations

Comparisons return Boolean arrays:

```python
a = np.array([2, 5, 8, 11])
a > 6
a == 5
```

Combine elementwise conditions with `&`, `|`, and `~`, using parentheses around comparisons:

```python
(a >= 5) & (a <= 10)
(a < 3) | (a > 9)
~(a > 6)
```

Use `np.logical_and`, `np.logical_or`, and `np.logical_not` when they make the intent clearer. Python's scalar `and` and `or` are not appropriate for multi-element Boolean arrays.

## 5. Aggregations and the `axis` argument

Common aggregations include `sum`, `mean`, `min`, `max`, `std`, `var`, `median`, and `argmax`.

```python
m = np.array([[1, 2, 3], [4, 5, 6]])
m.sum()          # 21
m.sum(axis=0)    # [5, 7, 9], sum down rows for each column
m.sum(axis=1)    # [6, 15], sum across columns for each row
```

For a 2D array:
- `axis=0` reduces the row dimension and returns one result per column.
- `axis=1` reduces the column dimension and returns one result per row.
- `keepdims=True` retains reduced axes with length one.

NumPy's default standard deviation uses `ddof=0`. Use `ddof=1` for the common sample standard deviation when appropriate.

## 6. Broadcasting

Broadcasting allows operations between arrays with compatible shapes without manually repeating data. Compare dimensions from right to left: dimensions must be equal or one of them must be 1; missing leading dimensions are treated as 1.

```python
column = np.array([[1], [2], [3]])       # (3, 1)
row = np.array([[10, 20, 30, 40]])       # (1, 4)
result = column + row                    # (3, 4)
```

Broadcasting is convenient but can create a large output array. Check expected shapes before operating on large data.

## 7. Reshaping, transposing, and flattening

```python
a = np.arange(12)
m = a.reshape(3, 4)
m.T
m.ravel()
m.flatten()
```

- `reshape()` changes shape while preserving the element sequence.
- `.T` reverses axis order for a 2D matrix (and reverses all axes for higher dimensions).
- `ravel()` returns a flattened view when possible, otherwise a copy.
- `flatten()` always returns a copy.

For higher-dimensional arrays, `np.transpose(a, axes=...)` can specify axis order explicitly.

## 8. Sorting, unique values, and searching

```python
a = np.array([4, 1, 4, 2, 1])
np.sort(a)             # sorted copy
np.unique(a)           # unique values
np.argsort(a)          # indices that would sort a
np.where(a > 2)        # tuple of matching indices
np.argmax(a)           # index of a maximum
```

`np.sort(a)` returns a sorted copy; `a.sort()` sorts the array in place. If several values tie for the maximum, `argmax` returns the first occurrence along the selected axis.

## 9. Combining and splitting arrays

```python
a = np.array([1, 2])
b = np.array([3, 4])
np.concatenate([a, b])
np.stack([a, b], axis=0)
np.vstack([a, b])
np.hstack([a, b])
np.split(np.arange(8), 4)
```

- `concatenate` joins along an existing axis.
- `stack` joins arrays along a new axis.
- `vstack` and `hstack` provide convenient vertical/horizontal layouts.
- `split` requires equal sections; `array_split` allows uneven sections.

Input shapes must be compatible for the chosen operation.

## 10. Matrix operations

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])
A @ B
np.linalg.det(A)
np.linalg.inv(A)
np.linalg.solve(A, np.array([5, 11]))
```

Use `np.linalg.solve(A, b)` to solve \(Ax=b\) when possible rather than explicitly computing an inverse. Inverses and determinants can be numerically sensitive; check whether a matrix is singular or ill-conditioned. Matrix multiplication shape rules differ from element-wise arithmetic.

## 11. Numerical care

- Floating-point arithmetic has finite precision; use `np.isclose` or `np.allclose` for approximate comparisons.
- Integer operations can overflow their fixed-width dtype.
- Division by zero can produce infinity or NaN in floating-point operations.
- Avoid modifying arrays unintentionally when operations return views.
- Prefer vectorized NumPy operations to explicit loops when they improve clarity.
- Inspect shapes and dtypes when debugging unexpected results.

## 12. Learning checklist

- [ ] Perform arithmetic and comparisons on arrays.
- [ ] Use common ufuncs such as `sqrt`, `exp`, `log`, and `sin`.
- [ ] Aggregate by axis and preserve dimensions with `keepdims`.
- [ ] Explain broadcasting compatibility.
- [ ] Reshape, transpose, sort, and search arrays.
- [ ] Combine and split arrays.
- [ ] Distinguish element-wise and matrix multiplication.
