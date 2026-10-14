# NumPy Array Indexing and Slicing

## 1. Overview

Indexing selects one or more elements from a NumPy array. Slicing selects a range of elements. These operations are essential for exploring datasets, selecting features, filtering observations, and preparing data for analysis.

Use `import numpy as np` for the examples.

## 2. Zero-based indexing

NumPy uses zero-based indexing: the first element is at index `0`, the second at index `1`, and so on.

```python
a = np.array([10, 20, 30, 40])
a[0]   # 10
a[2]   # 30
a[-1]  # 40, last element
a[-2]  # 30, second from last
```

Negative indices count backward from the end. Indexing with an invalid index raises `IndexError`.

## 3. Basic slicing

The general syntax is:

```python
array[start:stop:step]
```

- `start` is inclusive.
- `stop` is exclusive.
- `step` controls the increment and cannot be zero.
- Omitted values use defaults based on the step direction.

```python
a = np.array([0, 1, 2, 3, 4, 5, 6])
a[1:5]    # [1, 2, 3, 4]
a[:3]     # [0, 1, 2]
a[3:]     # [3, 4, 5, 6]
a[::2]    # [0, 2, 4, 6]
a[::-1]   # reversed view
```

A slice's stop index is not included. This makes adjacent slices such as `a[:3]` and `a[3:]` convenient for splitting an array without overlap.

## 4. Indexing two-dimensional arrays

For a 2D array, use `array[row, column]`.

```python
m = np.array([[10, 20, 30],
              [40, 50, 60],
              [70, 80, 90]])
m[0, 1]  # 20
m[2, 0]  # 70
m[1]     # second row
m[1, :]  # second row, all columns
m[:, 0]  # first column
m[:, -1] # last column
```

Rows are indexed first, columns second. In a 3D array, the indices correspond to each axis in order.

## 5. Slicing rows and columns

```python
m[:2, :2]    # first two rows and first two columns
m[1:, 1:]    # from row 1 and column 1 to the end
m[::2, ::2]  # every second row and column
```

A slice may be a view, so changing a selected element can change the original array.

## 6. Views versus copies

Basic slicing generally returns a **view** that refers to the same underlying data. Use `.copy()` if independent data is required.

```python
a = np.arange(5)
view = a[1:4]
view[0] = 99
print(a)  # [0, 99, 2, 3, 4]

b = np.arange(5)
copy = b[1:4].copy()
copy[0] = 99
print(b)  # [0, 1, 2, 3, 4]
```

Use `np.shares_memory(a, view)` to test whether two arrays share memory. Advanced indexing, such as integer-array indexing, generally returns a copy.

## 7. Fancy indexing with integer arrays

An integer index array selects elements in the requested order and may repeat indices.

```python
a = np.array([10, 20, 30, 40, 50])
a[[0, 2, 4]]   # [10, 30, 50]
a[[4, 1, 1]]   # [50, 20, 20]
```

For 2D arrays, paired integer index arrays select corresponding coordinate pairs:

```python
m = np.arange(9).reshape(3, 3)
m[[0, 2], [1, 0]]  # elements at (0, 1) and (2, 0)
```

This differs from selecting the Cartesian product of rows and columns. For a submatrix, use slices or `np.ix_`.

## 8. Boolean indexing

A Boolean mask selects elements where the mask is `True`.

```python
a = np.array([3, 8, 12, 5, 17])
a[a > 10]  # [12, 17]
```

Combine elementwise conditions using `&` and `|`, with parentheses around each comparison:

```python
a[(a >= 5) & (a <= 12)]
a[(a < 5) | (a > 12)]
```

Boolean indexing is useful for filtering values and rows. For a 2D dataset, `m[m[:, 1] >= 70]` selects rows where column 1 is at least 70.

## 9. Adding and removing dimensions

- `np.newaxis` or `None` adds a dimension.
- `np.expand_dims()` adds an axis at a chosen position.
- `np.squeeze()` removes axes of length one.

```python
a = np.array([1, 2, 3])
a[:, np.newaxis]  # shape (3, 1)
a[np.newaxis, :]  # shape (1, 3)
```

These operations are useful when aligning arrays for broadcasting.

## 10. Common pitfalls

1. The stop position in a slice is excluded.
2. Indexing order is row first, then column.
3. Basic slices can share memory with the original array.
4. Boolean masks must have compatible shapes.
5. Use `&` / `|`, not scalar `and` / `or`, for elementwise Boolean arrays.
6. Paired integer-array indices select coordinate pairs rather than a rectangular block.
7. Negative steps reverse traversal; the default start/stop behavior differs for negative steps.
8. Repeated indices in advanced assignment can have surprising behavior; avoid relying on an assumed update order.
9. An out-of-range index raises an error rather than returning a missing value.
10. Check `.shape` when slicing multidimensional data.

## 11. Learning checklist

- [ ] Select elements with positive and negative indices.
- [ ] Use `start:stop:step` slicing.
- [ ] Select rows, columns, and submatrices.
- [ ] Explain views and copies.
- [ ] Use integer-array indexing and Boolean masks.
- [ ] Add dimensions for broadcasting.
