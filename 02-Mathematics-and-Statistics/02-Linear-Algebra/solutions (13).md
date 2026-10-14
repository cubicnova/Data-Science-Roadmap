# Linear Algebra — Reference Solutions

```python
import numpy as np

# 1. Example: scalar 4; vector [1, 2]; matrix [[1, 2], [3, 4]].
# 2. Shape is (4, 3).
# 3.
v = np.array([2, 5, 8])

# 4.
a = np.array([1, 3, 5])
b = np.array([2, 4, 6])
print(a + b)  # [3 7 11]

# 5.
print(np.array([7, 8, 9]) - np.array([1, 2, 3]))  # [6 6 6]

# 6.
print(3 * np.array([2, -1, 4]))  # [6 -3 12]

# 7.
print(np.dot([1, 2, 3], [4, 5, 6]))  # 32

# 8. Each pair of corresponding components must exist; vector lengths must match.

# 9.
M = np.array([[1, 2, 3], [4, 5, 6]])
print(M.shape)  # (2, 3)

# 10.
A = np.array([[10, 20], [30, 40]])
print(A[1, 0])  # 30

# 11.
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])
print(A + B)  # [[6, 8], [10, 12]]

# 12.
print(np.array([[2, 4], [6, 8]]) * 0.5)

# 13. `*` is element-wise for compatible NumPy arrays; `@` performs
# matrix multiplication (or a dot product for 1-D vectors).

# 14.
A = np.array([[1, 2, 3], [4, 5, 6]])
B = np.array([[1, 2], [3, 4], [5, 6]])
print(A @ B)
# [[22, 28], [49, 64]], shape (2, 2)

# 15. Not defined: inner dimensions 3 and 4 do not match.

# 16.
print(np.array([[1, 2, 3], [4, 5, 6]]).T)
# [[1, 4], [2, 5], [3, 6]]

# 17.
print(np.eye(3))

# 18.
print(np.array([[1, 2], [3, 4]]) @ np.array([5, 6]))  # [17 39]

# 19. [7, 10] = 7[1, 0] + 10[0, 1].

# 20.
print(np.linalg.norm([3, 4]))  # 5.0

# 21.
print(np.abs([-2, 3, -5]).sum())  # 10

# 22.
print(np.linalg.norm(np.array([1, 2]) - np.array([4, 6])))  # 5.0

# 23. Add equations: 3x = 6, so x = 2; then y = 1.

# 24.
coefficients = np.array([[2, 1], [1, -1]])
constants = np.array([5, 1])
print(np.linalg.solve(coefficients, constants))  # [2. 1.]

# 25. Order generally matters: dimensions and entries of AB and BA may differ.

# 26.
A = np.array([[1, 2], [3, 4], [5, 6]])
print(A[1])  # [3 4]

# 27.
print(np.zeros((3, 2)))

# 28.
print(np.ones((2, 3)))

# 29.
print(np.dot([2, 3], [4, 5]))  # 23

# 30.
A = np.array([[1, 2, 3], [4, 5, 6]])
print(A.shape, A.T.shape)  # (2, 3), (3, 2)

# 31.
features = np.array([0.2, 0.7, 1.0])
weights = np.array([2.0, -1.0, 0.5])
print(features @ weights)  # 0.7

# 32. `*` multiplies corresponding entries; `@` combines rows and columns
# according to matrix multiplication rules.

# 33. (3, 2) @ (2, 4) is valid and yields shape (3, 4).

# 34.
def euclidean_distance(a, b):
    a = np.asarray(a, dtype=float)
    b = np.asarray(b, dtype=float)
    if a.shape != b.shape:
        raise ValueError("Vectors must have the same shape.")
    return float(np.linalg.norm(a - b))

# 35.
T = np.array([[1, 2], [3, 4]])
x = np.array([5, 6])
print(T @ x)  # [17 39]
```

## Challenge example

```python
A = np.array([[1, 2, 0], [0, 1, 3], [4, 0, 1]])
x = np.array([2, 1, 3])
product = A @ x
transpose = A.T

print(product)      # [4 10 11]
print(transpose.shape)  # (3, 3)
```

`A` is `(3, 3)`, `x` is `(3,)`, and the matrix-vector product is `(3,)`. The transpose remains `(3, 3)` because the original matrix is square.
