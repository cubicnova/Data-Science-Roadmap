# NumPy Cheatsheet

A practical NumPy reference for Python developers, data analysts, data scientists, and machine learning engineers.

NumPy (Numerical Python) is a fundamental Python library for numerical computing. It provides multidimensional arrays, vectorized operations, mathematical functions, statistical operations, and linear algebra tools.

## Table of Contents

- [1. Installation and Import](#1-installation-and-import)
- [2. Array Creation](#2-array-creation)
- [3. Array Attributes](#3-array-attributes)
- [4. Data Types](#4-data-types)
- [5. Indexing and Slicing](#5-indexing-and-slicing)
- [6. Array Operations](#6-array-operations)
- [7. Universal Functions](#7-universal-functions)
- [8. Reshaping Arrays](#8-reshaping-arrays)
- [9. Joining and Splitting Arrays](#9-joining-and-splitting-arrays)
- [10. Broadcasting](#10-broadcasting)
- [11. Aggregation and Statistics](#11-aggregation-and-statistics)
- [12. Boolean Indexing and Filtering](#12-boolean-indexing-and-filtering)
- [13. Sorting and Searching](#13-sorting-and-searching)
- [14. Copy vs View](#14-copy-vs-view)
- [15. Random Number Generation](#15-random-number-generation)
- [16. Linear Algebra](#16-linear-algebra)
- [17. Handling Missing and Infinite Values](#17-handling-missing-and-infinite-values)
- [18. Saving and Loading Arrays](#18-saving-and-loading-arrays)
- [19. Practical Data Science Examples](#19-practical-data-science-examples)
- [20. Performance Tips](#20-performance-tips)
- [21. Common Mistakes](#21-common-mistakes)
- [22. Quick Reference](#22-quick-reference)
- [23. Practice Exercises](#23-practice-exercises)
- [24. Official Resources](#24-official-resources)

## 1. Installation and Import

Install NumPy using pip:

```bash
pip install numpy
```

Import NumPy:

```python
import numpy as np

print(np.__version__)
```

The conventional alias `np` is used throughout the Python data science ecosystem.

## 2. Array Creation

### Create arrays from Python lists

```python
import numpy as np

# One-dimensional array
a = np.array([10, 20, 30, 40])

# Two-dimensional array
b = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

print(a)
print(b)
```

### Initialize arrays

```python
np.zeros((2, 3))          # Array filled with zeros
np.ones((2, 3))           # Array filled with ones
np.full((2, 3), 7)        # Array filled with 7
np.empty((2, 3))          # Uninitialized array
np.eye(3)                 # 3 × 3 identity matrix
```

`np.empty()` does not initialize elements to zero. Its contents are unspecified until you assign values.

### Create sequences

```python
np.arange(0, 10, 2)       # [0, 2, 4, 6, 8]
np.linspace(0, 1, 5)      # Five evenly spaced values
np.logspace(1, 3, 3)      # [10, 100, 1000]
```

### Create arrays from existing data

```python
a = np.array([1, 2, 3])

np.zeros_like(a)          # [0, 0, 0]
np.ones_like(a)           # [1, 1, 1]
np.full_like(a, 9)        # [9, 9, 9]
```

## 3. Array Attributes

```python
a = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

a.ndim       # Number of dimensions: 2
a.shape      # Dimensions: (2, 3)
a.size       # Total elements: 6
a.dtype      # Element data type
a.itemsize   # Bytes per element
a.nbytes     # Total bytes used by array elements
```

Useful conversions:

```python
a.astype(float)           # Convert element type
a.tolist()                # Convert to nested Python lists
a.flatten()               # Return a flattened copy
a.ravel()                 # Return a flattened array when possible as a view
```

## 4. Data Types

NumPy arrays generally store elements using a common data type.

```python
np.array([1, 2, 3], dtype=np.int32)
np.array([1, 2, 3], dtype=np.float64)
np.array([True, False], dtype=bool)
np.array([1 + 2j, 3 + 4j], dtype=complex)
```

Common types:

| Data type | Description |
|---|---|
| `np.int32` | 32-bit integer |
| `np.int64` | 64-bit integer |
| `np.float32` | 32-bit floating point |
| `np.float64` | 64-bit floating point |
| `np.bool_` | Boolean |
| `np.complex128` | Complex number |

Example:

```python
a = np.array([1, 2, 3])
b = a.astype(np.float64)

print(b)        # [1. 2. 3.]
print(b.dtype)
```

Use an appropriate data type to balance precision and memory consumption.

## 5. Indexing and Slicing

### One-dimensional indexing

```python
a = np.array([10, 20, 30, 40, 50])

a[0]             # 10
a[-1]            # 50
a[1:4]           # [20, 30, 40]
a[:3]            # [10, 20, 30]
a[::2]           # [10, 30, 50]
a[::-1]          # Reversed array
```

### Two-dimensional indexing

```python
a = np.array([
    [10, 20, 30],
    [40, 50, 60],
    [70, 80, 90]
])

a[0, 1]          # 20
a[1, :]          # Second row
a[:, 0]          # First column
a[0:2, 1:3]      # Subarray
a[-1, -1]        # 90
```

### Fancy indexing

```python
a = np.array([10, 20, 30, 40, 50])

a[[0, 2, 4]]     # [10, 30, 50]
```

Fancy indexing returns a copy rather than a view.

## 6. Array Operations

### Arithmetic operations

```python
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

a + b            # [5, 7, 9]
a - b            # [-3, -3, -3]
a * b            # [4, 10, 18]
a / b            # Element-wise division
a ** 2           # [1, 4, 9]
a % 2            # Remainders
```

### Scalar operations

```python
a = np.array([1, 2, 3])

a + 10           # [11, 12, 13]
a * 2            # [2, 4, 6]
a ** 3           # [1, 8, 27]
```

### Comparison operations

```python
a = np.array([10, 20, 30, 40])

a > 20           # [False, False, True, True]
a == 20          # [False, True, False, False]
a != 30          # Boolean array
```

### Matrix multiplication

```python
a = np.array([[1, 2], [3, 4]])
b = np.array([[5, 6], [7, 8]])

a @ b
np.matmul(a, b)
np.dot(a, b)
```

For two-dimensional matrices, `@` is the clearest operator for matrix multiplication. The `*` operator performs element-wise multiplication.

## 7. Universal Functions

Universal functions, or ufuncs, operate efficiently on array elements.

```python
a = np.array([1, 4, 9, 16])

np.sqrt(a)           # Square roots
np.square(a)         # Squares
np.abs([-1, -2, 3])  # Absolute values
np.exp(a)            # Exponential
np.log(a)            # Natural logarithm
np.log10(a)          # Base-10 logarithm
np.sin(a)            # Sine
np.cos(a)            # Cosine
np.round([1.23, 4.56], 1)
```

Other useful functions:

```python
np.maximum([1, 5, 3], [2, 4, 6])
np.minimum([1, 5, 3], [2, 4, 6])
np.clip([1, 5, 10], 2, 8)
```

`np.clip()` restricts values to a specified minimum and maximum.

## 8. Reshaping Arrays

```python
a = np.arange(12)

a.reshape(3, 4)       # 3 rows, 4 columns
a.reshape(2, 2, 3)    # Three-dimensional array
a.reshape(3, -1)      # Infer the second dimension
a.reshape(-1)         # One-dimensional array
```

### Transpose

```python
a = np.array([[1, 2, 3], [4, 5, 6]])

a.T
np.transpose(a)
```

### Add or remove dimensions

```python
a = np.array([1, 2, 3])

a[np.newaxis, :]      # Shape (1, 3)
a[:, np.newaxis]      # Shape (3, 1)

b = np.array([[[1, 2, 3]]])
np.squeeze(b)         # Remove dimensions of size 1
```

Reshaping requires the same total number of elements. A dimension of `-1` can be inferred automatically.

## 9. Joining and Splitting Arrays

### Concatenate arrays

```python
a = np.array([1, 2])
b = np.array([3, 4])

np.concatenate((a, b))
```

### Stack arrays

```python
np.stack((a, b))          # Add a new axis
np.vstack((a, b))         # Stack vertically
np.hstack((a, b))         # Stack horizontally
np.column_stack((a, b))   # Combine as columns
```

### Split arrays

```python
a = np.arange(12)

np.split(a, 3)            # Three equal sections
np.array_split(a, 5)      # Five sections, possibly unequal
np.hsplit(np.arange(12).reshape(3, 4), 2)
```

`np.split()` requires equal divisions when an integer number of sections is specified. Use `np.array_split()` when sections may differ in size.

## 10. Broadcasting

Broadcasting lets NumPy perform operations on compatible arrays without explicitly repeating all their elements.

```python
a = np.array([1, 2, 3])
print(a + 10)

# Add a row vector to each row
matrix = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

vector = np.array([10, 20, 30])

print(matrix + vector)
```

### Broadcasting rules

Compare dimensions from right to left. Two dimensions are compatible if they are equal or if either dimension is `1`. Missing dimensions are treated as `1`.

Example:

```python
a = np.array([[1], [2], [3]])  # Shape (3, 1)
b = np.array([10, 20, 30])     # Shape (3,)

result = a + b

print(result.shape)  # (3, 3)
```

The result is a 3 × 3 array because the shapes `(3, 1)` and `(3,)` broadcast to `(3, 3)`.

## 11. Aggregation and Statistics

```python
a = np.array([10, 20, 30, 40, 50])

np.sum(a)          # 150
np.mean(a)         # 30.0
np.median(a)       # 30.0
np.min(a)          # 10
np.max(a)          # 50
np.std(a)          # Population standard deviation
np.var(a)          # Population variance
np.percentile(a, 75)
np.quantile(a, 0.25)
np.argmin(a)       # Index of minimum
np.argmax(a)       # Index of maximum
np.ptp(a)          # Range: max - min
```

### Axis operations

```python
a = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

np.sum(a, axis=0)       # [5, 7, 9]: column sums
np.sum(a, axis=1)       # [6, 15]: row sums
np.mean(a, axis=0)      # Column means
np.max(a, axis=1)       # Maximum per row
```

For a two-dimensional array, `axis=0` reduces rows and returns results for each column; `axis=1` reduces columns and returns results for each row.

### Cumulative operations

```python
a = np.array([1, 2, 3, 4])

np.cumsum(a)        # [1, 3, 6, 10]
np.cumprod(a)       # [1, 2, 6, 24]
np.diff(a)          # [1, 1, 1]
```

## 12. Boolean Indexing and Filtering

```python
a = np.array([10, 15, 20, 25, 30])

a[a > 20]                       # [25, 30]
a[a % 2 == 0]                   # Even values
a[(a >= 15) & (a <= 25)]        # [15, 20, 25]
a[(a < 15) | (a > 25)]          # [10, 30]
```

Use parentheses around each condition when combining Boolean expressions.

### `where()` and `select()`

```python
a = np.array([10, 20, 30, 40])

np.where(a > 20, "High", "Low")
np.where(a > 20)                # Indices of matching elements
```

### Check conditions

```python
a = np.array([1, 2, 3])

np.any(a > 2)       # True if at least one matches
np.all(a > 0)       # True if all match
np.count_nonzero(a > 1)
```

## 13. Sorting and Searching

```python
a = np.array([30, 10, 40, 20])

np.sort(a)              # Sorted copy
np.argsort(a)           # Indices that sort the array
np.partition(a, 2)       # Partition around an index
np.unique(a)             # Unique values
np.searchsorted(np.sort(a), 25)
```

### Sorting by axis

```python
a = np.array([
    [3, 1, 2],
    [6, 4, 5]
])

np.sort(a, axis=0)       # Sort each column
np.sort(a, axis=1)       # Sort each row
np.sort(a, axis=None)    # Sort flattened values
```

`np.searchsorted()` is intended for sorted arrays and returns the insertion index that maintains sorted order.

## 14. Copy vs View

Understanding copies and views helps avoid accidental data modification.

### View

```python
a = np.array([1, 2, 3, 4])
b = a.view()

b[0] = 99

print(a)  # [99, 2, 3, 4]
```

A view shares the underlying data with its original array when applicable.

### Copy

```python
a = np.array([1, 2, 3, 4])
b = a.copy()

b[0] = 99

print(a)  # [1, 2, 3, 4]
```

A copy has its own array data.

### Slicing

```python
a = np.array([1, 2, 3, 4])
b = a[1:3]

b[0] = 99
print(a)  # [1, 99, 3, 4]
```

Basic slicing typically returns a view, whereas advanced indexing returns a copy. When independence matters, use `.copy()` explicitly.

## 15. Random Number Generation

Prefer NumPy's modern `default_rng()` interface for new code.

```python
rng = np.random.default_rng(seed=42)

rng.random(5)                  # Uniform values in [0, 1)
rng.integers(1, 10, size=5)    # Integers from 1 to 9
rng.normal(0, 1, size=5)       # Normal distribution
rng.uniform(0, 10, size=5)     # Uniform distribution
rng.choice([10, 20, 30], size=5)
rng.permutation(5)             # Random permutation
rng.shuffle(np.arange(5))      # Shuffles an array in place
```

For reproducible experiments, use a fixed seed:

```python
rng = np.random.default_rng(42)

sample = rng.normal(loc=100, scale=15, size=1000)

print(sample.mean())
print(sample.std())
```

A fixed seed makes the random sequence reproducible for the same generator and environment.

## 16. Linear Algebra

```python
a = np.array([[1, 2], [3, 4]])
b = np.array([[5, 6], [7, 8]])
```

### Matrix operations

```python
a @ b                       # Matrix multiplication
a.T                         # Transpose
np.linalg.det(a)            # Determinant
np.linalg.inv(a)            # Inverse, if nonsingular
np.linalg.matrix_rank(a)    # Matrix rank
np.trace(a)                 # Sum of diagonal elements
np.linalg.norm(a)           # Matrix norm
```

### Solve linear equations

For \(Ax=b\):

```python
A = np.array([
    [3, 1],
    [1, 2]
])

b = np.array([9, 8])

x = np.linalg.solve(A, b)
print(x)
```

Prefer `np.linalg.solve(A, b)` to explicitly computing `np.linalg.inv(A) @ b` when solving a square linear system.

### Eigenvalues and singular value decomposition

```python
values, vectors = np.linalg.eig(a)

U, S, Vh = np.linalg.svd(a)
```

SVD is widely used in dimensionality reduction, signal processing, and numerical analysis.

## 17. Handling Missing and Infinite Values

NumPy commonly represents missing numerical data with `np.nan`.

```python
a = np.array([10.0, np.nan, 30.0, 40.0])

np.isnan(a)              # Identify NaN
np.nanmean(a)            # Mean ignoring NaN
np.nansum(a)             # Sum ignoring NaN
np.nanmedian(a)          # Median ignoring NaN
```

### Replace invalid values

```python
a = np.array([1.0, np.nan, 3.0])

clean = np.nan_to_num(a, nan=0.0)
```

### Check infinite values

```python
a = np.array([1.0, np.inf, -np.inf, np.nan])

np.isinf(a)
np.isfinite(a)
np.isnan(a)
```

Note that `np.nan_to_num()` also replaces infinities by default. Specify `posinf` and `neginf` if you need particular replacement values.

## 18. Saving and Loading Arrays

### NumPy binary format

```python
a = np.arange(10)

np.save("array.npy", a)
loaded = np.load("array.npy")
```

### Save multiple arrays

```python
np.savez("arrays.npz", features=a, labels=a * 2)

data = np.load("arrays.npz")
features = data["features"]
labels = data["labels"]
```

### Text files

```python
a = np.array([[1, 2], [3, 4]])

np.savetxt("array.csv", a, delimiter=",", fmt="%d")
loaded = np.loadtxt("array.csv", delimiter=",")
```

For `.npz` files, `np.load()` returns an archive-like object. Close it when finished if you are working with files in long-running applications.

Only load NumPy object arrays from trusted sources. For untrusted files, use `np.load(..., allow_pickle=False)`.

## 19. Practical Data Science Examples

### Example 1: Normalize numerical features

Min-max normalization scales values to the range 0–1, provided the feature has a nonzero range.

```python
import numpy as np

features = np.array([10.0, 20.0, 30.0, 40.0])

minimum = features.min()
maximum = features.max()

normalized = (features - minimum) / (maximum - minimum)

print(normalized)
# [0.         0.33333333 0.66666667 1.        ]
```

For a constant-valued feature, the denominator is zero; handle that case explicitly.

### Example 2: Standardize features

```python
features = np.array([10.0, 20.0, 30.0, 40.0])

mean = features.mean()
std = features.std()

standardized = (features - mean) / std

print(standardized)
```

This example uses the population standard deviation (`ddof=0`). Standardization is commonly applied before training machine learning models.

### Example 3: Calculate student averages

```python
marks = np.array([
    [80, 90, 85],
    [70, 75, 80],
    [95, 92, 98]
])

student_average = marks.mean(axis=1)
subject_average = marks.mean(axis=0)

print("Student averages:", student_average)
print("Subject averages:", subject_average)
```

### Example 4: Detect outliers using the IQR rule

```python
data = np.array([10, 12, 13, 14, 15, 16, 100])

q1, q3 = np.percentile(data, [25, 75])
iqr = q3 - q1

lower = q1 - 1.5 * iqr
upper = q3 + 1.5 * iqr

outliers = data[(data < lower) | (data > upper)]

print(outliers)
```

The IQR rule flags potential outliers; it does not prove that the observations are erroneous.

### Example 5: Compute pairwise differences

```python
values = np.array([100, 120, 115, 140])

changes = np.diff(values)

print(changes)
# [ 20  -5  25]
```

This technique is useful for examining changes in time series and sequential measurements.

## 20. Performance Tips

1. **Prefer vectorization.** Use `a * 2` instead of looping through each element in Python.
2. **Use broadcasting.** Avoid unnecessarily repeating arrays with `np.tile()` or `np.repeat()`.
3. **Choose appropriate data types.** `float32` may reduce memory use when its precision is sufficient.
4. **Avoid unnecessary copies.** Basic slicing often returns a view.
5. **Preallocate arrays.** Use `np.zeros()` or `np.empty()` when the final size is known.
6. **Use `out=` where appropriate.** Some ufuncs can write results into an existing array.
7. **Profile before optimizing.** Measure runtime and memory use on representative data.
8. **Use specialized tools when needed.** Large distributed arrays or GPU workloads may be better suited to libraries such as Dask, CuPy, or JAX.

Example:

```python
a = np.arange(1_000_000)

# Vectorized operation
result = a * 2

# Preallocated output
output = np.empty_like(a)
np.multiply(a, 2, out=output)
```

## 21. Common Mistakes

| Mistake | Better approach |
|---|---|
| Using `*` for matrix multiplication | Use `@` |
| Mixing incompatible shapes | Inspect `.shape` and broadcasting rules |
| Modifying a slice unintentionally | Use `.copy()` when independent data is needed |
| Using `np.empty()` as a zero-filled array | Use `np.zeros()` |
| Dividing by a zero range during normalization | Handle constant-valued features |
| Using `and` or `or` with array comparisons | Use `&` or `\|` with parentheses |
| Loading untrusted pickled arrays | Use `allow_pickle=False` |
| Assuming every array operation changes the original | Check whether it returns a new array, view, or in-place result |

Example of combining Boolean conditions correctly:

```python
a = np.array([10, 20, 30, 40])

mask = (a >= 20) & (a <= 30)

print(a[mask])
# [20 30]
```

## 22. Quick Reference

| Task | NumPy syntax |
|---|---|
| Create array | `np.array([1, 2, 3])` |
| Zeros | `np.zeros((2, 3))` |
| Ones | `np.ones((2, 3))` |
| Range | `np.arange(0, 10, 2)` |
| Even spacing | `np.linspace(0, 1, 5)` |
| Shape | `a.shape` |
| Dimensions | `a.ndim` |
| Data type | `a.dtype` |
| Reshape | `a.reshape(2, 3)` |
| Flatten | `a.ravel()` |
| Transpose | `a.T` |
| Sum | `np.sum(a)` |
| Mean | `np.mean(a)` |
| Standard deviation | `np.std(a)` |
| Unique values | `np.unique(a)` |
| Filter values | `a[a > 10]` |
| Conditional values | `np.where(a > 10, 1, 0)` |
| Join arrays | `np.concatenate((a, b))` |
| Sort | `np.sort(a)` |
| Random values | `np.random.default_rng(42)` |
| Matrix multiplication | `a @ b` |
| Solve a system | `np.linalg.solve(A, b)` |
| Save an array | `np.save("array.npy", a)` |
| Load an array | `np.load("array.npy")` |

## 23. Practice Exercises

Try these exercises to reinforce the concepts.

1. Create a NumPy array containing integers from 1 to 20.
2. Reshape the array into a 4 × 5 matrix.
3. Extract the second row and third column.
4. Find all even numbers in the original array.
5. Calculate the mean, median, variance, and standard deviation.
6. Replace all values greater than 10 with zero.
7. Normalize an array of numerical features between 0 and 1.
8. Combine two arrays horizontally and vertically.
9. Generate 100 random numbers from a normal distribution and calculate their mean.
10. Solve a system of two linear equations using `np.linalg.solve()`.
11. Identify potential outliers using the IQR method.
12. Demonstrate the difference between a view and a copy.

### Mini Project: Student Performance Analyzer

Build a small analysis program using NumPy.

Requirements:

- Store marks for multiple students and subjects in a two-dimensional array.
- Calculate each student's average.
- Calculate each subject's average.
- Find the student with the highest overall average.
- Identify students whose average is below a selected threshold.
- Normalize subject marks for comparison.

This mini project reinforces array creation, indexing, aggregation, filtering, and vectorized calculations.

## 24. Official Resources

- [NumPy Official Documentation](https://numpy.org/doc/)
- [NumPy Beginner's Guide](https://numpy.org/doc/stable/user/absolute_beginners.html)
- [NumPy Reference](https://numpy.org/doc/stable/reference/)
- [NumPy Tutorials](https://numpy.org/numpy-tutorials/)
- [NumPy Quickstart Tutorial](https://numpy.org/doc/stable/user/quickstart.html)

---

**Next step:** Practice these commands in a Jupyter Notebook, then apply NumPy to data cleaning, exploratory data analysis, and feature preparation before moving on to Pandas and Scikit-learn.


