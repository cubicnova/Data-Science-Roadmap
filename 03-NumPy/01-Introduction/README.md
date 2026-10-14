# Introduction to NumPy

## 1. What is NumPy?

NumPy (Numerical Python) is a foundational Python library for numerical computing. Its central data structure is the `ndarray`, an efficient multidimensional array that supports vectorized operations, indexing, reshaping, aggregation, and linear algebra.

NumPy is widely used in data science, scientific computing, machine learning, statistics, image processing, and data preprocessing.

## 2. Why use NumPy instead of Python lists?

- **Vectorization:** apply operations to whole arrays without writing explicit Python loops.
- **Multidimensional arrays:** represent vectors, matrices, and higher-dimensional data.
- **Broadcasting:** perform operations between compatible shapes.
- **Numerical functions:** access statistics, random sampling, linear algebra, and more.
- **Memory and performance:** homogeneous arrays are often more compact and faster for numerical work than general-purpose lists.

Python lists remain useful for general collections and mixed data types. NumPy arrays are especially useful for numerical data.

## 3. Installation and import

Install NumPy in the active Python environment:

```bash
python -m pip install numpy
```

Import it using the conventional alias:

```python
import numpy as np
print(np.__version__)
```

In Jupyter, ensure the notebook kernel uses the same environment in which NumPy was installed.

## 4. Creating arrays

```python
import numpy as np

a = np.array([10, 20, 30])
matrix = np.array([[1, 2, 3], [4, 5, 6]])
zeros = np.zeros((2, 3))
ones = np.ones((2, 2))
sequence = np.arange(0, 10, 2)
evenly_spaced = np.linspace(0, 1, 5)
```

Common constructors:

| Function | Purpose |
|---|---|
| `np.array()` | Create an array from a sequence |
| `np.zeros(shape)` | Fill an array with zeros |
| `np.ones(shape)` | Fill an array with ones |
| `np.full(shape, value)` | Fill with a specified value |
| `np.arange(start, stop, step)` | Generate values using a step |
| `np.linspace(start, stop, num)` | Generate a fixed number of evenly spaced values |
| `np.eye(n)` | Create an identity matrix |
| `np.empty(shape)` | Allocate an array without initializing its values |

Use `np.empty` only when every element will be assigned before it is read.

## 5. Array attributes

For an array `a`:

- `a.ndim`: number of dimensions
- `a.shape`: size along each dimension
- `a.size`: total number of elements
- `a.dtype`: element data type
- `a.itemsize`: bytes per element (for the array's dtype)

Example:

```python
a = np.array([[1, 2, 3], [4, 5, 6]])
print(a.ndim)   # 2
print(a.shape)  # (2, 3)
print(a.size)   # 6
print(a.dtype)  # integer dtype, platform-dependent
```

## 6. Data types and conversion

NumPy arrays usually store values of one common dtype. Common types include `int32`, `int64`, `float32`, `float64`, and `bool_`; exact defaults can depend on platform and construction.

```python
values = np.array([1, 2, 3], dtype=np.float64)
integers = values.astype(np.int64)
```

Casting can lose information. For example, converting floating-point values to integers truncates fractional parts rather than rounding them.

## 7. Vectorized arithmetic

```python
a = np.array([1, 2, 3])
b = np.array([10, 20, 30])

a + b       # element-wise addition
a * b       # element-wise multiplication
a ** 2      # element-wise square
a + 5       # scalar broadcasting
```

The `*` operator is element-wise multiplication, not matrix multiplication. Use `@` or `np.matmul()` for matrix multiplication.

## 8. Indexing and slicing

For a one-dimensional array:

```python
a = np.array([10, 20, 30, 40, 50])
a[0]       # first element
a[-1]      # last element
a[1:4]     # elements at indices 1, 2, 3
a[::2]     # every second element
```

For a 2D array:

```python
m = np.array([[1, 2, 3], [4, 5, 6]])
m[0, 1]    # row 0, column 1 -> 2
m[:, 0]    # first column
m[1, :]    # second row
```

Basic slices commonly return **views**, so editing a slice can modify the original array. Use `.copy()` when independent data is required. Advanced indexing, such as integer-array or Boolean-array indexing, generally returns a copy.

## 9. Reshaping and dimensions

```python
a = np.arange(12)
m = a.reshape(3, 4)
flat = m.ravel()
column = a.reshape(-1, 1)
```

A reshape must preserve the total number of elements. The `-1` dimension asks NumPy to infer the compatible size.

- `reshape()` changes shape when possible.
- `ravel()` returns a flattened view when possible, otherwise a copy.
- `flatten()` always returns a copy.
- `np.expand_dims()` adds a dimension.
- `np.squeeze()` removes dimensions of length one.

## 10. Aggregations and statistics

```python
a = np.array([[1, 2, 3], [4, 5, 6]])
np.sum(a)
np.mean(a)
np.min(a)
np.max(a)
np.std(a)
np.median(a)
```

For a 2D array, `axis=0` aggregates down rows and returns one result per column; `axis=1` aggregates across columns and returns one result per row.

NumPy's default standard deviation uses `ddof=0` (population-style denominator). For the common sample standard deviation, use `np.std(a, ddof=1)` when appropriate and when enough observations exist.

## 11. Comparisons and Boolean masks

```python
a = np.array([4, 9, 12, 3])
mask = a > 8
selected = a[mask]
```

Boolean masks are useful for filtering data. Combine elementwise conditions with parentheses and `&` or `|`:

```python
(a > 5) & (a < 15)
```

Do not use Python's scalar `and` / `or` with arrays of multiple Boolean values.

## 12. Broadcasting

Broadcasting allows elementwise operations on arrays with compatible shapes. Compare shapes from right to left: each pair of dimensions must be equal or one of them must be 1; missing leading dimensions are treated as 1.

Example: an array of shape `(3, 1)` plus one of shape `(1, 4)` produces shape `(3, 4)`.

Broadcasting avoids manually repeating data and often saves memory.

## 13. Random number generation

Prefer a dedicated generator:

```python
rng = np.random.default_rng(42)
samples = rng.normal(loc=0, scale=1, size=5)
integers = rng.integers(0, 10, size=5)
```

A fixed seed makes a sequence reproducible in a given environment and NumPy version, but exact streams are not guaranteed to remain identical across all future versions or methods.

## 14. Common mistakes and best practices

1. Check `shape` and `dtype` when debugging unexpected results.
2. Distinguish elementwise `*` from matrix multiplication `@`.
3. Remember that the stop value of `np.arange` is excluded.
4. Avoid equality checks on floating-point values when tolerance-based comparisons are more appropriate; use `np.isclose` or `np.allclose`.
5. Watch for integer overflow and unintended dtype conversion.
6. Know when slicing produces a view and use `.copy()` when needed.
7. Use broadcasting and vectorization for clear numerical code.
8. Avoid creating enormous arrays accidentally; estimate `size * itemsize` before allocation.
9. Set a random seed or generator for reproducible examples and experiments.
10. Use `axis` deliberately and verify the output shape.

## 15. Learning checklist

- [ ] Explain what NumPy is used for.
- [ ] Create 1D, 2D, and 3D arrays.
- [ ] Inspect `ndim`, `shape`, `size`, and `dtype`.
- [ ] Use indexing, slicing, and Boolean masks.
- [ ] Perform vectorized arithmetic.
- [ ] Reshape arrays and explain views versus copies.
- [ ] Compute aggregations with `axis`.
- [ ] Explain broadcasting and generate random data.
