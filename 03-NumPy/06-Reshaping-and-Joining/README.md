# NumPy Reshaping and Joining

Learn how to change array shapes and combine arrays without changing their underlying data unnecessarily.

## Learning objectives
By the end of this module, you should be able to:
- Understand an array's `shape`, `ndim`, and `size`.
- Reshape arrays when the element count remains unchanged.
- Flatten or ravel arrays and understand the copy/view distinction.
- Add or remove dimensions with `np.newaxis`, `expand_dims`, and `squeeze`.
- Combine arrays with `concatenate`, `stack`, `hstack`, `vstack`, and `dstack`.
- Split arrays with `split`, `array_split`, `hsplit`, and `vsplit`.
- Diagnose common shape and axis errors.

## 1. Shape fundamentals

```python
import numpy as np

a = np.arange(12)
print(a.shape)  # (12,)
print(a.ndim)   # 1
print(a.size)   # 12
```

- **`shape`**: tuple giving the length of each dimension.
- **`ndim`**: number of dimensions.
- **`size`**: total number of elements.

For an array of shape `(r, c)`, the element count is `r * c`. For shape `(a, b, c)`, it is `a * b * c`.

## 2. Reshaping arrays

`reshape` changes the dimensions while preserving the number and order of elements.

```python
a = np.arange(12)
matrix = a.reshape(3, 4)
print(matrix)
```

The target dimensions must multiply to the original number of elements. Use `-1` for one dimension that NumPy should infer:

```python
a.reshape(2, -1)  # shape (2, 6)
a.reshape(-1, 3)  # shape (4, 3)
```

Only one dimension may be `-1`. A reshape may return a view when the memory layout permits it, or a copy otherwise. Do not assume modifying the result always modifies the original.

## 3. Flattening: `flatten` and `ravel`

```python
a = np.arange(6).reshape(2, 3)
a.flatten()  # always returns a copy
a.ravel()    # returns a view when possible; otherwise a copy
```

Both return a one-dimensional sequence of elements. `ravel` can be more memory-efficient when a view is possible. The optional `order` argument controls traversal order, such as `"C"` (row-major) or `"F"` (column-major).

## 4. Adding and removing dimensions

```python
a = np.array([10, 20, 30])
a[np.newaxis, :].shape       # (1, 3)
a[:, np.newaxis].shape       # (3, 1)
np.expand_dims(a, axis=0).shape  # (1, 3)
```

Use `np.squeeze` to remove axes of length one:

```python
x = np.zeros((1, 3, 1))
np.squeeze(x).shape  # (3,)
np.squeeze(x, axis=0).shape  # (3, 1)
```

An axis can only be squeezed if its length is one.

## 5. Joining arrays

### `concatenate`
Joins arrays along an existing axis. All other dimensions must match.

```python
a = np.array([[1, 2], [3, 4]])
b = np.array([[5, 6]])
np.concatenate((a, b), axis=0)
```

### `stack`
Creates a new axis, so all input arrays must have the same shape.

```python
a = np.array([1, 2])
b = np.array([3, 4])
np.stack((a, b), axis=0).shape  # (2, 2)
np.stack((a, b), axis=1).shape  # (2, 2)
```

The resulting values differ by axis even when the shape is the same.

### Convenience functions
- `np.vstack`: stack rows; 1-D inputs are treated as rows.
- `np.hstack`: join horizontally; for 1-D inputs, concatenate end-to-end.
- `np.dstack`: join along the third axis, promoting lower-dimensional inputs as needed.

For predictable behavior with complex shapes, prefer `concatenate` or `stack` and specify the axis explicitly.

## 6. Splitting arrays

```python
a = np.arange(12)
left, right = np.split(a, 2)
```

`np.split` requires an even division when an integer number of sections is given. `np.array_split` allows uneven sections:

```python
np.array_split(np.arange(10), 3)
```

For 2-D arrays, `np.hsplit` splits columns and `np.vsplit` splits rows. The chosen axis must be divisible for equal splitting.

## 7. Choosing the correct operation

| Goal | Operation | Key rule |
|---|---|---|
| Change dimensions | `reshape` | Element count must match |
| Make 1-D copy | `flatten` | Always copies |
| Flatten efficiently | `ravel` | View when possible |
| Join along existing axis | `concatenate` | Other dimensions match |
| Join using new axis | `stack` | Shapes must match |
| Split evenly | `split` | Divisible section lengths |
| Split unevenly | `array_split` | Uneven lengths allowed |
| Add singleton axis | `expand_dims` / `newaxis` | Adds a dimension |
| Remove singleton axes | `squeeze` | Removed axes must have length 1 |

## 8. Common errors and best practices

1. Check `array.shape` before reshaping or joining.
2. Never reshape to a target whose dimensions imply a different element count.
3. Remember that `axis=0` refers to rows for a 2-D array, while `axis=1` refers to columns.
4. Use `concatenate` when joining along an existing axis; use `stack` when creating a new axis.
5. Validate shapes before combining data from different sources.
6. Prefer descriptive intermediate variables over deeply nested array operations.
7. Use `np.array_split` when equal-sized chunks are not required.
8. Test whether memory sharing matters before modifying reshaped arrays.

## Quick reference

```python
a.reshape(3, 4)
a.reshape(-1, 2)
a.flatten()
a.ravel()
np.expand_dims(a, axis=0)
np.squeeze(a)
np.concatenate((a, b), axis=0)
np.stack((a, b), axis=0)
np.split(a, 3)
np.array_split(a, 3)
```

## Further reading
- [NumPy `reshape`](https://numpy.org/doc/stable/reference/generated/numpy.reshape.html)
- [NumPy `concatenate`](https://numpy.org/doc/stable/reference/generated/numpy.concatenate.html)
- [NumPy `stack`](https://numpy.org/doc/stable/reference/generated/numpy.stack.html)
- [NumPy `split`](https://numpy.org/doc/stable/reference/generated/numpy.split.html)
