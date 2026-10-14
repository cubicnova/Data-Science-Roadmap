# NumPy Introduction — Practice Questions

There are 35 questions, moving from fundamentals to practical coding. Use `import numpy as np`.

## A. Fundamentals

1. What is NumPy, and what does `ndarray` mean?
2. Give three benefits of NumPy arrays compared with Python lists for numerical computing.
3. Write the command to install NumPy with pip.
4. Import NumPy using the standard alias and print its version.
5. Create a NumPy array containing the integers 5, 10, 15, 20, and 25.
6. Create a 2 × 3 array filled with zeros.
7. Create a 3 × 2 array filled with ones.
8. Generate the even numbers from 0 up to, but not including, 20 using `np.arange`.
9. Generate seven evenly spaced values between 0 and 1 using `np.linspace`.
10. Create a 4 × 4 identity matrix.

## B. Attributes, types, and arithmetic

11. Create a 2D array and print its `ndim`, `shape`, `size`, and `dtype`.
12. Create an array of floating-point values with dtype `np.float32`.
13. Convert an array of floating-point values to integers using `astype`; describe what happens to fractional parts.
14. Given `a = np.array([2, 4, 6])` and `b = np.array([1, 3, 5])`, calculate `a + b`, `a - b`, `a * b`, and `a ** 2`.
15. Add 10 to every element of an array without writing a loop.
16. Create a 3 × 3 array containing values 1 through 9 and find its sum, mean, minimum, and maximum.
17. For a 2D array, calculate the sum for each column and for each row using `axis`.
18. Calculate the population standard deviation and sample standard deviation of `[2, 4, 6, 8]`.

## C. Indexing, slicing, and shape

19. Retrieve the first and last elements of a 1D array.
20. Extract the elements at indices 2 through 5 from an array using slicing.
21. From a 3 × 4 matrix, select the element in row 2, column 1 using zero-based indexing.
22. Select the first column and last row of a 2D array.
23. Create an array of 12 values and reshape it into 3 rows and 4 columns.
24. Reshape an array into a column vector using `-1`.
25. Flatten a 2D array with `ravel()` and compare it with `flatten()`.
26. Demonstrate that modifying a basic slice may change its original array; then show how `.copy()` avoids that shared-data behavior.

## D. Filtering and broadcasting

27. Select all values greater than 10 from a NumPy array using a Boolean mask.
28. Select values between 5 and 15, inclusive, using a combined Boolean condition.
29. Explain why `(a > 5) & (a < 15)` needs parentheses.
30. Add an array of shape `(3, 1)` to an array of shape `(1, 4)` and report the resulting shape.
31. Normalize a non-constant numeric array to the range 0 to 1 using `(x - x.min()) / (x.max() - x.min())`. What should you consider if all values are equal?

## E. Random data and mini tasks

32. Create a random number generator with seed 42 and generate five integers from 0 through 9.
33. Generate 1,000 samples from a normal distribution with mean 10 and standard deviation 2. Compute their sample mean and standard deviation.
34. Create a small array of exam scores and calculate the mean score, the count of scores at least 70, and the scores at least 70.
35. Write a short program that creates a 2D dataset, inspects its shape and dtype, filters rows or values using a condition, computes column-wise means, and prints the results.

## Submission expectations

For coding questions, submit runnable code and its output. Add a one-sentence explanation for questions involving `axis`, broadcasting, random generation, and views/copies.
