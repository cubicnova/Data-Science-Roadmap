# NumPy Broadcasting — Practice Questions

Use `import numpy as np`. For each shape question, explain how dimensions align from right to left.

## A. Rules and shape prediction

1. Define broadcasting in NumPy.
2. State the two rules that make dimensions compatible.
3. Predict the result shape of `(3, 4) + (4,)`.
4. Predict the result shape of `(3, 4) + (3, 1)`.
5. Predict the result shape of `(3, 1) + (1, 4)`.
6. Are shapes `(3, 4)` and `(3,)` compatible for element-wise addition? Explain.
7. Are shapes `(2, 3, 4)` and `(3, 1)` compatible? State the result shape.
8. Are shapes `(5, 1, 4)` and `(1, 3, 4)` compatible? State the result shape.
9. Predict the result shape when a scalar is added to an array of shape `(2, 3, 4)`.
10. Explain how NumPy aligns a 1D array with a 2D array.

## B. Practical broadcasting

11. Add 10 to every element in a 2 × 3 array.
12. Add `[10, 20, 30]` to every row of a 2 × 3 matrix.
13. Add `[100, 200]` to the rows of a 2 × 3 matrix using a column vector.
14. Multiply a 3 × 1 array by a 1 × 4 array and inspect the result.
15. Subtract a per-column offset from a 4 × 3 dataset.
16. Apply a different scale factor to each column of a 3 × 4 matrix.
17. Compare every value in a matrix with a 1D per-column threshold array.
18. Use `np.where` with a broadcastable threshold array to replace values below their column threshold.
19. Create arrays with shapes `(2, 1, 3)` and `(1, 4, 1)`, add them, and report the output shape.
20. Use `np.newaxis` to convert a vector of shape `(5,)` into shapes `(1, 5)` and `(5, 1)`.

## C. Normalization and data preparation

21. Standardize each column of a 2D dataset using its column means and standard deviations.
22. Explain why `X.mean(axis=0)` broadcasts naturally across rows of `X`.
23. Write a safe standardization function that avoids division by zero for constant columns.
24. Normalize each row independently to the range 0 to 1 using broadcasting.
25. Explain why explicit tiling is usually unnecessary for adding a row vector to a matrix.

## D. Debugging and memory

26. Diagnose the shape mismatch between `(4, 3)` and `(4,)`. Show a corrected expression for one value per row.
27. Diagnose the shape mismatch between `(2, 3)` and `(2,)`. Show a corrected expression for one value per row.
28. Use `np.broadcast_to` to create a read-only view of a row with shape `(3,)` broadcast to `(4, 3)`.
29. Explain why the result of `np.broadcast_to` should not be modified in place.
30. Estimate the memory for a float64 result of shape `(1000, 2000)` in bytes and MiB (using 1 MiB = 1,048,576 bytes).

## E. Applied tasks

31. A class has marks in three subjects for four students. Add subject-wise bonus values using broadcasting.
32. Add a different offset to each of the 5 rows of a `(5, 3)` dataset.
33. Given a `(100, 6)` feature matrix, subtract the six feature means without a loop.
34. Create two arrays with shapes `(2, 1, 3)` and `(1, 4, 1)`, multiply them, and explain the result shape.
35. Write a small function that checks if two shapes are broadcast-compatible and, if so, returns the expected output shape. You may use NumPy's `np.broadcast_shapes`.

## Submission guidelines

Include code and output for practical tasks. Clearly label array shapes and explain the alignment for at least five questions.
