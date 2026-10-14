# NumPy Array Indexing and Slicing — Reference Solutions

Use `import numpy as np`. Equivalent correct solutions are acceptable.

## A. Basic indexing

1. `a = np.array([10, 20, 30, 40, 50]); print(a[0])` → `10`
2. `print(a[2])` → `30`
3. `print(a[-1], a[-2])` → `50 40`
4. NumPy raises `IndexError`.
5. `a = np.arange(10); print(a[[2, 5, 8]])` → `[2 5 8]`

## B. Slicing one-dimensional arrays

6. `np.arange(10)[2:7]` → `[2 3 4 5 6]`
7. `a[:4]`
8. `a[5:]`
9. `a[::2]`
10. `a[::-1]`
11. `a[:5]` and `a[5:]`
12. `a[:4]` selects indices 0–3; `a[4:]` selects index 4 through the end.
13. `a[2:12:3]` selects indices 2, 5, 8, and 11.

## C. Two-dimensional indexing and slicing

14. For `m = np.arange(1, 10).reshape(3, 3)`, `m[1, 2]` is `6`.
15. `m[1, :]` or `m[1]`
16. `m[:, 0]`
17. `m[:, -1]`
18. `m[:2, :2]`
19. `m[-2:, -2:]` for a 4 × 4 matrix.
20. `m[::2, :]`
21. `m[:, ::2]`
22. `m[[0, 2], [2, 1]]` selects values at coordinate pairs `(0, 2)` and `(2, 1)`.
23.
```python
m = np.arange(9).reshape(3, 3)
rows = [0, 2]
cols = [1, 2]
print(m[np.ix_(rows, cols)])
```
This returns a 2 × 2 submatrix containing the selected rows and columns.

## D. Views, copies, and masks

24.
```python
a = np.arange(5)
view = a[1:4]
view[0] = 99
print(a)  # [0 99 2 3 4]
```
25.
```python
a = np.arange(5)
copy = a[1:4].copy()
copy[0] = 99
print(a)  # [0 1 2 3 4]
```
26.
```python
a = np.arange(6)
view = a[1:4]
selected = a[[1, 2, 3]]
print(np.shares_memory(a, view))      # True
print(np.shares_memory(a, selected))  # False
```
Basic slicing generally gives a view; advanced integer indexing generally returns a copy.
27. `a[a > 10]`
28. `a[(a >= 5) & (a <= 15)]`
29. `data[data[:, 1] >= 70]`
30. Parentheses ensure each comparison is evaluated first; `&` and `|` combine the resulting Boolean arrays element by element.

## E. Practical tasks

31. For `m = np.arange(20).reshape(5, 4)`, `m[1:4, 1:3]`
32. `a[:, np.newaxis]` or `np.expand_dims(a, axis=1)`
33. `a[np.newaxis, :]` or `np.expand_dims(a, axis=0)`
34.
```python
def even_index_values(a):
    return np.asarray(a)[::2]
```
35.
```python
data = np.array([
    [18, 70, 1],
    [21, 82, 0],
    [19, 65, 1],
    [25, 91, 0],
])
selected_rows = data[data[:, 1] >= 70]
final = selected_rows[:, [0, 1]]
print(final)
print(final.shape)
```
The final shape is `(3, 2)` because three rows meet the score condition and two columns are selected.
