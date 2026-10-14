# NumPy Arrays and Data Types — Practice Questions

Use `import numpy as np`. Show your code and outputs for coding questions.

## A. Arrays and attributes

1. Define a NumPy `ndarray`.
2. Create a 0D array containing 12 and print its shape and number of dimensions.
3. Create a 1D array containing numbers 1 through 8.
4. Create a 3 × 4 matrix containing the numbers 0 through 11.
5. Create a 3D array with shape `(2, 3, 4)` filled with zeros.
6. For a 3 × 4 array, print its `shape`, `ndim`, and `size`.
7. Explain the relationship between `shape` and `size`.
8. Create an array with dtype `np.int16` and print its dtype and item size.
9. Print the strides of a 2D array and explain what they represent.
10. Use `.item()` to extract a Python scalar from a 0D array.

## B. Data types and ranges

11. Create arrays using `np.int8`, `np.uint8`, `np.float32`, `np.float64`, and `np.bool_`.
12. Use `np.iinfo` to display the minimum and maximum values of `np.int8`.
13. Use `np.iinfo` to inspect the range of `np.uint8`.
14. Use `np.finfo` to compare the precision of `np.float32` and `np.float64`.
15. Explain when a signed integer is more appropriate than an unsigned integer.
16. Create an array from `[1, 2.5, 3]` and inspect the inferred dtype.
17. Create a string array and explain what can happen when strings of different lengths are stored in a fixed-width string dtype.
18. Create a Boolean mask from a numeric array.

## C. Type conversion and precision

19. Convert `[1.2, 2.8, -3.7]` to integers with `astype`. Explain the result.
20. Round those values to the nearest integer before converting them to an integer dtype.
21. Demonstrate a potentially lossy conversion by converting a value outside the range of `uint8`; explain why the result should not be relied on as a way to preserve the original value.
22. Convert an integer array to `float32` and inspect the new dtype.
23. Explain the difference between `reshape()` and `astype()`.
24. Compare `np.asarray()` and `np.array()` at a high level.
25. Test whether converting a floating-point array to integers returns an independent array using `np.shares_memory`.

## D. Missing values and memory

26. Create a floating-point array containing `np.nan`, positive infinity, and a finite value.
27. Identify NaN values using `np.isnan`.
28. Identify finite values using `np.isfinite`.
29. Explain why `x == np.nan` is not the correct way to detect missing values.
30. Calculate the `nbytes` of arrays with 100 elements of `float32` and `float64`. Explain the difference.
31. Create a slice and a copy of an array, mutate both, and observe which mutation changes the original.

## E. Applied tasks

32. Create an array of image-like pixel values from 0 to 255 using `uint8`; explain why range validation matters.
33. Create a matrix with `float32` dtype and reshape it without changing its dtype.
34. Write a function that checks whether all values in a numeric array are finite.
35. Write a short report comparing two dtypes for a small dataset: range, precision, memory use, and whether conversion loses information.

## Submission guidelines

Include runnable Python code, output, and brief explanations for type conversion, memory, and missing-value questions.
