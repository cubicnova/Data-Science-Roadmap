# NumPy Arrays and Data Types

## 1. Learning objectives

By the end of this module, you should be able to create NumPy arrays, inspect their dimensions and shapes, select appropriate dtypes, convert between types, understand memory implications, and avoid common casting mistakes.

## 2. The `ndarray` structure

A NumPy `ndarray` is a multidimensional, generally homogeneous array. Its key properties are:

- **Shape:** tuple describing the number of elements along each axis.
- **Dimensions (`ndim`):** number of axes.
- **Size:** total number of elements, equal to the product of shape dimensions.
- **Dtype:** data type used to represent the array's elements.
- **Strides:** byte steps used to move along each axis in memory.

Example:

```python
import numpy as np

a = np.array([[1, 2, 3], [4, 5, 6]], dtype=np.int32)
print(a.shape)    # (2, 3)
print(a.ndim)     # 2
print(a.size)     # 6
print(a.dtype)    # int32
print(a.itemsize) # 4 bytes per element
```

The exact integer dtype inferred by NumPy can depend on the input and platform. Specify a dtype explicitly when it matters.

## 3. Array dimensions

```python
scalar = np.array(7)                         # 0D, shape ()
vector = np.array([1, 2, 3])                 # 1D, shape (3,)
matrix = np.array([[1, 2], [3, 4]])          # 2D, shape (2, 2)
cube = np.zeros((2, 3, 4))                   # 3D, shape (2, 3, 4)
```

A Python scalar and a zero-dimensional NumPy array are related but are not the same object. Use `.item()` to extract a Python scalar from a single-element NumPy array when appropriate.

## 4. Common NumPy dtypes

| Category | Examples | Notes |
|---|---|---|
| Signed integers | `np.int8`, `np.int16`, `np.int32`, `np.int64` | Represent negative and positive integers within a fixed range |
| Unsigned integers | `np.uint8`, `np.uint16`, `np.uint32`, `np.uint64` | Represent non-negative integers within a fixed range |
| Floating point | `np.float16`, `np.float32`, `np.float64` | Approximate real values with different precision/range trade-offs |
| Complex | `np.complex64`, `np.complex128` | Store real and imaginary components |
| Boolean | `np.bool_` | `True` or `False` |
| Text | `np.str_` | Unicode string values |
| Bytes | `np.bytes_` | Byte-string values |
| Date/time | `np.datetime64`, `np.timedelta64` | Dates/times and time differences |
| Object | `object` | References to Python objects; often less efficient for numerical arrays |

### Integer ranges

For an integer dtype, inspect limits instead of memorizing them:

```python
np.iinfo(np.int8)
np.iinfo(np.uint8)
```

For floating-point limits and precision:

```python
np.finfo(np.float32)
np.finfo(np.float64)
```

## 5. Choosing a dtype

Choose the smallest dtype that safely represents the data and provides adequate precision. For example, `uint8` is often used for pixel values from 0 to 255, while `float32` is common in large numerical and machine-learning workloads. Do not choose a dtype based on size alone: range, precision, and downstream operations matter.

## 6. Type inference and explicit dtype

```python
a = np.array([1, 2, 3])
b = np.array([1.0, 2.5, 3.0])
c = np.array([1, 2, 3], dtype=np.float32)
```

Mixed numeric sequences are promoted to a common dtype so values can be represented consistently. Explicit dtypes improve clarity and can control memory use.

## 7. Type conversion with `astype`

```python
floats = np.array([1.2, 2.8, -3.7])
ints = floats.astype(np.int64)
print(ints)  # [ 1  2 -3]
```

Floating-point-to-integer conversion truncates toward zero; it does not round to the nearest integer. If rounding is intended, use `np.rint`, `np.floor`, or `np.ceil` before converting, depending on the desired rule.

Casting can also overflow or lose precision. For example, converting 300 to `uint8` cannot represent 300. Avoid assuming every cast is lossless.

## 8. Shape changes are different from dtype changes

- `reshape()` changes dimensions without changing the logical sequence of values.
- `astype()` converts the element representation and normally returns a new array.
- `np.asarray()` converts input to an array where possible and may avoid a copy.
- `np.array()` usually creates an array and defaults to copying input data, though exact behavior depends on parameters and NumPy version.

Example:

```python
a = np.arange(6, dtype=np.int32)
b = a.reshape(2, 3)
c = a.astype(np.float64)
```

`b` can share memory with `a`; `c` is a converted array.

## 9. Missing and non-finite values

Floating-point arrays can represent `np.nan`, `np.inf`, and `-np.inf`. Integer arrays do not directly represent `NaN`. Use `np.isnan`, `np.isfinite`, and appropriate missing-value conventions.

```python
x = np.array([1.0, np.nan, np.inf])
print(np.isnan(x))
print(np.isfinite(x))
```

Do not compare `NaN` with equality (`x == np.nan`); use `np.isnan(x)`.

## 10. Memory use

For a contiguous array, the data buffer size is approximately `a.nbytes`, equal to `a.size * a.itemsize`. This does not include all Python object and metadata overhead. Object arrays can have different memory characteristics.

```python
a = np.zeros(1_000, dtype=np.float64)
print(a.nbytes)
```

A `float64` element uses 8 bytes; a `float32` element uses 4 bytes. Use lower precision only when it is safe for the task.

## 11. Views, copies, and contiguity

Basic slicing often returns a view that shares data with the original. `copy()` creates independent data. Reshape and transpose can often be views, depending on strides and layout.

```python
a = np.arange(6)
view = a[1:4]
copy = a[1:4].copy()
```

Use `np.shares_memory(a, view)` to test whether two arrays share memory. Be cautious when mutating views.

## 12. Best practices

1. Inspect `shape`, `ndim`, and `dtype` when data behaves unexpectedly.
2. Specify dtypes for file formats, memory-sensitive workflows, and model inputs.
3. Validate ranges before casting to narrow or unsigned integer types.
4. Do not assume floating-point calculations are exact.
5. Use `np.isclose` / `np.allclose` for tolerance-based floating-point comparisons.
6. Handle `NaN` and infinity deliberately.
7. Distinguish shape operations from type conversions.
8. Test casts on boundary values.
9. Check `nbytes` before allocating large arrays.
10. Prefer clear, explicit code over clever implicit conversions.

## 13. Learning checklist

- [ ] Explain `shape`, `ndim`, `size`, `dtype`, `strides`, and `itemsize`.
- [ ] Create scalar, vector, matrix, and higher-dimensional arrays.
- [ ] Identify common numeric and non-numeric dtypes.
- [ ] Inspect integer and floating-point limits.
- [ ] Convert types and identify lossy conversions.
- [ ] Recognize `NaN`, infinity, views, and copies.
