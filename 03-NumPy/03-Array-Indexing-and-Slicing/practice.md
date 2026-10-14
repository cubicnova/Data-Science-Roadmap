# NumPy Array Indexing and Slicing — Practice Questions

Use `import numpy as np`. For coding questions, show the code and resulting output.

## A. Basic indexing

1. Create an array containing `[10, 20, 30, 40, 50]` and retrieve the first element.
2. Retrieve the third element of the same array.
3. Retrieve the last and second-last elements using negative indexing.
4. What error occurs when you try to access an index outside an array's valid range?
5. Create an array from 0 through 9 and retrieve elements at indices 2, 5, and 8 using integer-array indexing.

## B. Slicing one-dimensional arrays

6. From `np.arange(10)`, select elements from index 2 up to, but not including, index 7.
7. Select the first four elements.
8. Select all elements from index 5 to the end.
9. Select every second element.
10. Reverse an array using slicing.
11. Split an array of 10 elements into its first five and last five elements without overlap.
12. Explain the difference between `a[:4]` and `a[4:]`.
13. From an array of 12 elements, select indices 2, 5, 8, and 11 using a slice if possible.

## C. Two-dimensional indexing and slicing

14. Create a 3 × 3 matrix containing values 1 through 9. Retrieve the element at row 1, column 2 using zero-based indexing.
15. Select the second row of a matrix.
16. Select the first column of a matrix.
17. Select the last column of a matrix.
18. Select the top-left 2 × 2 submatrix of a 3 × 3 matrix.
19. Select the last two rows and last two columns of a 4 × 4 matrix.
20. Select every second row from a 2D array.
21. Select every second column from a 2D array.
22. Select the element at row 0, column 2, and row 2, column 1 using paired integer index arrays.
23. Use `np.ix_` to select the Cartesian product of rows `[0, 2]` and columns `[1, 2]`.

## D. Views, copies, and masks

24. Create a slice of an array, modify the slice, and demonstrate that the original array changes.
25. Repeat Question 24 using `.copy()` and show that the original remains unchanged.
26. Use `np.shares_memory` to compare an array with a basic slice and with an advanced-indexed result.
27. Select all values greater than 10 from an array.
28. Select values between 5 and 15, inclusive, using a Boolean mask.
29. Given a matrix where the second column contains scores, select rows with scores of at least 70.
30. Explain why Boolean array conditions should use parentheses with `&` or `|`.

## E. Practical tasks

31. Given a 5 × 4 matrix, extract its middle three rows and middle two columns.
32. Add a new axis to a 1D array so that its shape becomes `(n, 1)`.
33. Add a new axis to a 1D array so that its shape becomes `(1, n)`.
34. Write a function that returns all values at even indices from a 1D array.
35. Write a short program that creates a 2D dataset, selects rows based on a condition, selects chosen columns, and prints the final shape.

## Submission guidelines

Include runnable code and outputs. Add a short explanation for Questions 22–26 and for any task involving array shapes.
