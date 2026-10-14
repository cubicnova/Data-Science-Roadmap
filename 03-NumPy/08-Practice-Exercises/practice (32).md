# Practice Exercises — NumPy

Complete these 35 exercises in order. They range from array basics to small data-analysis challenges.

1. **Create an array:** Create a NumPy array containing integers from 1 through 10.

2. **Use arange:** Create all even integers from 2 through 20 using `np.arange`.

3. **Use linspace:** Create 6 equally spaced values from 0 to 1.

4. **Inspect metadata:** For `np.arange(12).reshape(3, 4)`, print shape, dimensions, size, and dtype.

5. **Create zeros and ones:** Create a 2 × 3 array of zeros and a 3 × 2 array of ones.

6. **Create an identity matrix:** Create a 4 × 4 identity matrix.

7. **Index one element:** Get the element in row 2, column 3 of a 4 × 5 array containing values 0–19. Use zero-based indexing.

8. **Select a row:** Select the second row from a 3 × 4 matrix.

9. **Select a column:** Select the last column from a 3 × 4 matrix.

10. **Slice a submatrix:** From a 5 × 5 matrix, select the central 3 × 3 block.

11. **Reverse an array:** Reverse the values in a 1-D array using slicing.

12. **Step slicing:** Select every second element from `np.arange(20)`.

13. **Update selected values:** In an array from 1 to 10, replace values greater than 6 with 0.

14. **Boolean filtering:** Extract all values divisible by 3 from 1 through 30.

15. **Fancy indexing:** From `np.arange(10)`, select elements at indices 1, 4, and 7.

16. **Elementwise arithmetic:** Add, subtract, multiply, and divide two arrays of equal length.

17. **Vectorized transformation:** Square every element in a 3 × 3 array without using a loop.

18. **Aggregate values:** Calculate sum, minimum, maximum, and mean for `np.arange(1, 11)`.

19. **Reduce by axis:** For a 3 × 4 matrix, calculate row sums and column sums.

20. **Broadcast a scalar:** Multiply a 2 × 3 array by 10.

21. **Broadcast vectors:** Add a length-3 vector to every row of a 2 × 3 matrix.

22. **Normalize values:** Subtract the mean from every element of an array.

23. **Reshape values:** Reshape 24 values into a 2 × 3 × 4 array.

24. **Flatten a matrix:** Flatten a 3 × 4 matrix into a 1-D array.

25. **Add an axis:** Convert a length-5 vector into a column vector with shape `(5, 1)`.

26. **Concatenate arrays:** Join two 2 × 2 arrays vertically.

27. **Stack arrays:** Stack two length-4 arrays along a new axis and inspect the shape.

28. **Split an array:** Split 18 values into 3 equal parts.

29. **Find statistical summaries:** For `[12, 15, 18, 21, 24, 30]`, calculate median, standard deviation, and the 75th percentile.

30. **Handle NaN:** Calculate the mean of `[2.0, 4.0, np.nan, 8.0]` while ignoring NaN.

31. **Find values by condition:** Use `np.where` to label temperatures above 30 as `1` and the rest as `0` for `[28, 32, 35, 27, 31]`.

32. **Random reproducibility:** Create two random integer arrays using separate generators initialized with the same seed and verify equality.

33. **Count condition matches:** Count how many values in a 4 × 5 matrix are greater than or equal to 10.

34. **Mini-project: marks analysis:** Create a 5 × 3 marks matrix. Calculate each subject's mean, the highest mark per student, and the overall median.

35. **Mini-project: sales analysis:** Given a 7 × 3 sales matrix (days × products), calculate total sales per product, best sales day, and the day with the lowest total.

