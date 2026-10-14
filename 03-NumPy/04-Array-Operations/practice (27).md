# NumPy Array Operations — Practice Questions

Use `import numpy as np`. Include runnable code and output for programming tasks.

## A. Arithmetic and universal functions

1. Create arrays `[2, 4, 6]` and `[1, 3, 5]`. Calculate their sum, difference, product, and quotient.
2. Square every element of an array without using a loop.
3. Calculate the square root of `[1, 4, 9, 16]`.
4. Apply `np.abs()` to an array containing negative and positive values.
5. Calculate the exponential and natural logarithm of suitable positive values.
6. Create an array of angles in radians and compute their sine and cosine.
7. Explain the difference between element-wise multiplication and matrix multiplication.
8. Compare two arrays and return a Boolean array indicating which elements of the first are greater.
9. Select values that are between 5 and 15, inclusive, using Boolean operations.
10. Use `np.where()` to replace values below 50 with 0 while retaining other values.

## B. Aggregations and axes

11. Calculate the sum, mean, minimum, maximum, variance, and standard deviation of an array.
12. For a 3 × 4 matrix, calculate the sum of each column.
13. Calculate the mean of each row in a matrix.
14. Use `keepdims=True` to calculate row means and inspect the result shape.
15. Find the index of the maximum and minimum elements in a 1D array.
16. Calculate the median of an array containing an even number of values.
17. Explain the difference between `np.std(x, ddof=0)` and `np.std(x, ddof=1)`.

## C. Broadcasting and shape operations

18. Add a `(3, 1)` array to a `(1, 4)` array and report the result shape.
19. Add a scalar to every element in a 2D array.
20. Explain why arrays of shapes `(3, 2)` and `(3,)` cannot be broadcast together.
21. Reshape 12 values into a 3 × 4 matrix and transpose it.
22. Flatten a matrix using both `ravel()` and `flatten()` and describe their copy behavior.
23. Normalize a non-constant array to the range 0 to 1. Handle the constant-array case safely.

## D. Sorting, combining, and splitting

24. Sort an array in ascending order and then in descending order.
25. Return the unique values of an array and the indices that would sort it.
26. Use `np.where()` to find the indices of values greater than a threshold.
27. Concatenate two 1D arrays.
28. Stack two equal-length 1D arrays to create a 2D array.
29. Split an array of 12 values into four equal sections.
30. Explain when `np.array_split()` is preferable to `np.split()`.

## E. Matrix operations and mini tasks

31. Multiply two 2 × 2 matrices using `@`.
32. Solve the linear system `2x + y = 5` and `x + 3y = 6` using `np.linalg.solve`.
33. Compute the determinant of a 2 × 2 matrix.
34. Write a function that returns a dictionary containing an array's mean, minimum, maximum, and standard deviation.
35. Create a small dataset of student marks. Use vectorized operations to add 5 bonus points, cap scores at 100, compute the mean, and identify students scoring at least 75.

## Submission guidelines

Show the code, output, and a short explanation for axis operations, broadcasting, and matrix calculations.
