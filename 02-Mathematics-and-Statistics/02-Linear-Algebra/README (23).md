# Linear Algebra for Data Science

Linear algebra studies vectors, matrices, and linear transformations. It is essential in data science because datasets can be represented as tables and many machine-learning calculations use vector and matrix operations.

## Learning objectives

- Understand scalars, vectors, matrices, and their dimensions.
- Perform vector addition, scalar multiplication, dot products, and matrix operations.
- Understand transpose, identity matrices, and matrix multiplication.
- Interpret linear combinations, linear systems, and matrix-vector products.
- Calculate norms and understand geometric meaning.
- Perform common linear-algebra operations with Python and NumPy.

## 1. Scalars, vectors, and matrices

- **Scalar:** a single number, such as \(5\).
- **Vector:** an ordered collection of numbers, such as \(\mathbf{v}=[2,4,6]\).
- **Matrix:** a rectangular array of numbers, such as

\[
A=\begin{bmatrix}1&2&3\\4&5&6\end{bmatrix}
\]

The matrix above has 2 rows and 3 columns, so its shape is \(2\times3\). A vector with three entries can be represented as a \(3\times1\) column vector or a \(1\times3\) row vector, depending on context.

## 2. Vector operations

Vectors of the same length can be added or subtracted component by component:

\[
[1,2,3]+[4,5,6]=[5,7,9]
\]

Scalar multiplication multiplies every component:

\[
3[1,2,3]=[3,6,9]
\]

## 3. Dot product

For vectors of equal length,

\[
\mathbf{a}\cdot\mathbf{b}=\sum_{i=1}^{n}a_i b_i
\]

Example:

\[
[1,2,3]\cdot[4,5,6]=1(4)+2(5)+3(6)=32
\]

The dot product produces a scalar. It is used in weighted sums, similarity calculations, and linear models.

## 4. Matrix dimensions and indexing

A matrix with \(m\) rows and \(n\) columns has shape \((m,n)\). In Python's NumPy, indexing starts at zero:

```python
import numpy as np

A = np.array([[1, 2, 3], [4, 5, 6]])
print(A.shape)  # (2, 3)
print(A[0, 1])  # 2
```

The expression `A[0, 1]` accesses the first row and second column.

## 5. Matrix addition and scalar multiplication

Matrices can be added only when their shapes match:

\[
\begin{bmatrix}1&2\\3&4\end{bmatrix}
+
\begin{bmatrix}5&6\\7&8\end{bmatrix}
=
\begin{bmatrix}6&8\\10&12\end{bmatrix}
\]

Scalar multiplication multiplies every entry. NumPy supports these operations directly for arrays of matching shape.

## 6. Matrix multiplication

If \(A\) has shape \(m\times n\) and \(B\) has shape \(n\times p\), then \(AB\) is defined and has shape \(m\times p\). The inner dimensions must match.

For example, a \(2\times3\) matrix can multiply a \(3\times2\) matrix, producing a \(2\times2\) result.

Matrix multiplication is not generally commutative: \(AB\ne BA\), and sometimes only one of the two products is defined.

In NumPy, use `A @ B` or `np.matmul(A, B)` for matrix multiplication. The `*` operator performs element-wise multiplication when the arrays have compatible shapes.

## 7. Transpose and identity matrix

The transpose swaps rows and columns. If \(A\) has shape \(2\times3\), then \(A^T\) has shape \(3\times2\).

An identity matrix \(I\) has ones on its main diagonal and zeros elsewhere. For compatible dimensions, \(AI=A\) and \(IA=A\).

## 8. Linear combinations and matrix-vector multiplication

A linear combination combines vectors using scalar weights:

\[
\mathbf{w}=c_1\mathbf{v}_1+c_2\mathbf{v}_2
\]

Matrix-vector multiplication performs weighted sums of the vector entries for each matrix row. It appears in linear regression, neural networks, and transformations of features.

## 9. Norms and distance

The Euclidean norm (length) of a vector is:

\[
\|\mathbf{x}\|_2=\sqrt{x_1^2+x_2^2+\cdots+x_n^2}
\]

For \([3,4]\), the Euclidean norm is \(\sqrt{3^2+4^2}=5\).

The Euclidean distance between vectors \(\mathbf{x}\) and \(\mathbf{y}\) is \(\|\mathbf{x}-\mathbf{y}\|_2\). The L1 norm is the sum of absolute component values.

## 10. Solving a linear system

A system of linear equations can be written as \(A\mathbf{x}=\mathbf{b}\). For example:

\[
\begin{aligned}
2x+y&=5\\
x-y&=1
\end{aligned}
\]

The solution is \(x=2,\ y=1\). In numerical work, use a linear solver such as `numpy.linalg.solve` when the coefficient matrix is square and nonsingular. Avoid explicitly computing an inverse when solving a system.

## 11. NumPy examples

```python
import numpy as np

a = np.array([1, 2, 3])
b = np.array([4, 5, 6])
print(a + b)
print(a @ b)

A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])
print(A + B)
print(A @ B)
print(A.T)
print(np.linalg.norm(np.array([3, 4])))

coefficients = np.array([[2, 1], [1, -1]])
constants = np.array([5, 1])
print(np.linalg.solve(coefficients, constants))
```

## 12. Best practices

- Check shapes before performing matrix multiplication.
- Use `@` for matrix multiplication and `*` for element-wise multiplication.
- Prefer NumPy arrays for numerical vector and matrix operations.
- Use `numpy.linalg.solve` for well-posed square systems rather than calculating an inverse solely to solve them.
- Floating-point calculations can have small rounding errors.
- Be explicit about whether a vector is a row or column vector when discussing dimensions.

## Quick reference

| Operation | Meaning |
|---|---|
| `a + b` | Element-wise vector addition |
| `3 * a` | Scalar multiplication |
| `a @ b` | Dot product for 1-D vectors; matrix multiplication for 2-D arrays |
| `A.T` | Transpose |
| `A.shape` | Array dimensions |
| `np.linalg.norm(a)` | Euclidean norm by default |
| `np.linalg.solve(A, b)` | Solve \(Ax=b\) for a suitable square matrix |

## Summary

Vectors and matrices provide a compact way to represent data and express calculations. Understanding dimensions, dot products, matrix multiplication, norms, and linear systems builds a foundation for statistics and machine learning.
