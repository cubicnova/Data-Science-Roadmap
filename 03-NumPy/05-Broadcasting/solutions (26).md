# NumPy Broadcasting — Reference Solutions

Use `import numpy as np`. Equivalent correct solutions are acceptable.

## A. Rules and shape prediction

1. Broadcasting lets element-wise operations work on arrays of compatible but potentially different shapes.
2. Aligned dimensions must be equal or one of them must be 1; missing leading dimensions are treated as 1.
3. `(3, 4)` — `(4,)` aligns as `(1, 4)`.
4. `(3, 4)` — `(3, 1)` aligns as `(3, 1)`, so result `(3, 4)`.
5. Result `(3, 4)`.
6. No. Align `(3, 4)` with `(1, 3)`; trailing dimensions 4 and 3 are incompatible.
7. Yes. Align `(2, 3, 4)` with `(1, 3, 1)`, result `(2, 3, 4)`.
8. Yes. `(5, 1, 4)` and `(1, 3, 4)` produce `(5, 3, 4)`.
9. `(2, 3, 4)`; a scalar effectively has shape `()`.
10. A 1D shape `(n,)` aligns with the last axis of a multidimensional array, not automatically with rows.

## B. Practical broadcasting

11. `a + 10`
12. `matrix + np.array([10, 20, 30])`
13. `matrix + np.array([100, 200])[:, np.newaxis]`
14.
```python
a = np.array([[1], [2], [3]])
b = np.array([[10, 20, 30, 40]])
result = a * b
print(result.shape)  # (3, 4)
```
15. `data - offsets`, where `offsets.shape == (3,)`.
16. `matrix * scales`, where `scales.shape == (4,)`.
17. `matrix > thresholds`, where `thresholds.shape` equals the number of columns.
18. `np.where(matrix < thresholds, 0, matrix)`
19. Output shape `(2, 4, 3)`: align `(2, 1, 3)` and `(1, 4, 1)`.
20. `v[np.newaxis, :]` and `v[:, np.newaxis]`.

## C. Normalization and data preparation

21.
```python
means = X.mean(axis=0)
stds = X.std(axis=0)
safe_stds = np.where(stds == 0, 1, stds)
Z = (X - means) / safe_stds
```
22. The mean array has shape `(number_of_columns,)`, which aligns with the last dimension of `X`.
23.
```python
def standardize_columns(X):
    X = np.asarray(X, dtype=float)
    means = X.mean(axis=0)
    stds = X.std(axis=0)
    safe_stds = np.where(stds == 0, 1.0, stds)
    return (X - means) / safe_stds
```
Constant columns become zero after centering and division by 1.
24.
```python
mins = X.min(axis=1, keepdims=True)
maxs = X.max(axis=1, keepdims=True)
span = maxs - mins
safe_span = np.where(span == 0, 1, span)
normalized = (X - mins) / safe_span
```
25. NumPy automatically broadcasts the row vector. Tiling allocates repeated data unnecessarily and makes the code less direct.

## D. Debugging and memory

26. `(4, 3)` and `(4,)` are incompatible because 3 and 4 differ. For one value per row, use `values[:, np.newaxis]` with shape `(4, 1)`.
27. `(2, 3)` and `(2,)` are incompatible because 3 and 2 differ. For one value per row, use `values[:, None]`.
28.
```python
row = np.array([1, 2, 3])
view = np.broadcast_to(row, (4, 3))
print(view.shape)  # (4, 3)
print(view.flags.writeable)  # False
```
29. The broadcasted result can expose the same underlying memory in multiple positions, so it is read-only by default to prevent ambiguous writes.
30. `1000 * 2000 * 8 = 16,000,000` bytes; `16,000,000 / 1,048,576 ≈ 15.26 MiB`. This is the data buffer only, not all overhead.

## E. Applied tasks

31.
```python
marks = np.array([[70, 80, 90], [60, 75, 85], [88, 92, 79], [65, 70, 72]])
bonus = np.array([2, 0, 5])
adjusted = marks + bonus
```
32.
```python
data = np.zeros((5, 3))
row_offsets = np.array([1, 2, 3, 4, 5])
adjusted = data + row_offsets[:, None]
```
33. `X_centered = X - X.mean(axis=0)`
34. Shapes `(2, 1, 3)` and `(1, 4, 1)` multiply to `(2, 4, 3)` because the singleton axes expand to compatible sizes.
35.
```python
def broadcast_result_shape(shape_a, shape_b):
    return np.broadcast_shapes(shape_a, shape_b)

print(broadcast_result_shape((3, 1), (1, 4)))  # (3, 4)
```
`np.broadcast_shapes` raises `ValueError` for incompatible shapes; optionally catch it to return a message.
