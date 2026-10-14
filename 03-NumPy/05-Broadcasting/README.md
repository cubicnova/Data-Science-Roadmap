# NumPy Broadcasting

## 1. What is broadcasting?

Broadcasting is NumPy's mechanism for applying element-wise operations to arrays with compatible shapes. It allows arrays of different shapes to interact without manually copying or repeating values.

```python
import numpy as np

a = np.array([[1], [2], [3]])          # shape (3, 1)
b = np.array([[10, 20, 30, 40]])       # shape (1, 4)
result = a + b                         # shape (3, 4)
```

The result is:

```text
[[11 21 31 41]
 [12 22 32 42]
 [13 23 33 43]]
```

## 2. The broadcasting rules

Compare shapes from **right to left**. Two dimensions are compatible when:

1. They are equal, or
2. One of them is `1`.

If one shape has fewer dimensions, pad it on the left with dimensions of size `1`. If any aligned pair of dimensions is incompatible, the operation raises a broadcasting `ValueError`.

Examples:

| Shape A | Shape B | Result | Compatible? |
|---|---|---|---|
| `(3, 4)` | `(3, 4)` | `(3, 4)` | Yes |
| `(3, 4)` | `(4,)` | `(3, 4)` | Yes |
| `(3, 4)` | `(3, 1)` | `(3, 4)` | Yes |
| `(3, 1)` | `(1, 4)` | `(3, 4)` | Yes |
| `(3, 4)` | `(3,)` | — | No |
| `(2, 3, 4)` | `(3, 1)` | `(2, 3, 4)` | Yes |

For `(3, 4)` and `(3,)`, the aligned trailing dimensions are `4` and `3`; neither is 1, so they are incompatible. To apply one value per row, use a column vector of shape `(3, 1)`.

## 3. Scalars and one-dimensional arrays

A scalar broadcasts across every element:

```python
x = np.array([[1, 2], [3, 4]])
x + 10
```

A 1D array aligns with the **last axis**:

```python
x = np.zeros((2, 3))
x + np.array([10, 20, 30])  # one value per column
```

To apply one value per row, reshape the 1D array:

```python
row_values = np.array([100, 200])
x + row_values[:, np.newaxis]  # one value per row
```

## 4. `None`, `np.newaxis`, and `reshape`

These operations can add dimensions without changing the underlying values:

```python
v = np.array([1, 2, 3])
v.shape                   # (3,)
v[np.newaxis, :].shape    # (1, 3)
v[:, np.newaxis].shape    # (3, 1)
v.reshape(3, 1).shape     # (3, 1)
np.expand_dims(v, axis=0).shape  # (1, 3)
```

Choose a shape based on the axis that should align during broadcasting.

## 5. Broadcasting with common operations

Broadcasting works with many element-wise operations, including addition, subtraction, multiplication, comparisons, `np.where`, and universal functions.

```python
scores = np.array([[70, 80, 90], [60, 75, 85]])
curve = np.array([5, 0, -5])
adjusted = scores + curve
```

For matrix data, broadcasting can apply per-column offsets, per-row thresholds, or normalization parameters.

## 6. Normalization example

For each feature column, subtract its mean and divide by its standard deviation:

```python
X = np.array([[1.0, 10.0], [2.0, 20.0], [3.0, 30.0]])
means = X.mean(axis=0)          # shape (2,)
stds = X.std(axis=0)            # shape (2,)
Z = (X - means) / stds
```

The `(2,)` means and standard deviations broadcast across the rows of `X`. If a column has zero standard deviation, define a safe policy rather than dividing by zero.

## 7. Broadcasting versus repeating

`np.broadcast_to` creates a read-only broadcasted view where possible; it does not generally allocate a full writable copy. `np.tile` repeats data into a new array and can use much more memory. Usually, let arithmetic broadcast naturally rather than explicitly tiling inputs.

```python
row = np.array([1, 2, 3])
view = np.broadcast_to(row, (4, 3))
```

Do not try to modify a `broadcast_to` result in place; it is read-only by default.

## 8. Memory and performance

Broadcasting often avoids materializing repeated inputs, but the **result** of an operation may still be large. Before applying operations to large arrays, inspect the resulting shape and estimate memory requirements.

```python
shape = (1000, 2000)
estimated_bytes_float64 = shape[0] * shape[1] * np.dtype(np.float64).itemsize
```

Broadcasting is not a reason to ignore output size. Use chunking or other approaches when a full result is too large.

## 9. Common mistakes

- Forgetting that alignment begins at the trailing dimension.
- Assuming a shape `(n,)` means a column vector; it is a 1D array.
- Trying to combine incompatible shapes.
- Adding dimensions on the wrong axis.
- Using `np.tile` when ordinary broadcasting would work.
- Accidentally producing a large output array.
- Dividing by a zero standard deviation during normalization.

## 10. Learning checklist

- [ ] State the two core broadcasting compatibility rules.
- [ ] Predict result shapes without running code.
- [ ] Explain why `(m, n)` and `(m,)` may be incompatible.
- [ ] Use `np.newaxis` and `reshape` to align dimensions.
- [ ] Apply per-row and per-column operations.
- [ ] Normalize data with broadcasting.
- [ ] Distinguish broadcasting from tiling and estimate output memory.
