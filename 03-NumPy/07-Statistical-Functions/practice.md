# Practice Exercises — NumPy Statistical Functions

Complete these 35 exercises. Try writing the code yourself before checking `solutions.md`.

1. **Mean:** Calculate the mean of `[10, 20, 30, 40, 50]`.

2. **Median, odd length:** Find the median of `[9, 2, 7, 4, 5]`.

3. **Median, even length:** Find the median of `[10, 20, 30, 40]`.

4. **Mean versus median:** For `[10, 12, 13, 15, 100]`, calculate mean and median. Which is less affected by the outlier?

5. **Minimum and maximum:** Find the minimum and maximum in `[8, 3, 12, 6, 9]`.

6. **Range:** Use `np.ptp` to calculate the range of `[14, 3, 22, 9]`.

7. **Index of minimum:** Find the index of the smallest value in `[6, 2, 9, 1, 7]`.

8. **Index of maximum:** Find the index of the largest value in `[6, 2, 9, 1, 7]`.

9. **Population variance:** Calculate variance using the default `ddof=0` for `[2, 4, 6, 8]`.

10. **Sample variance:** Calculate sample variance for `[2, 4, 6, 8]` using `ddof=1`.

11. **Population standard deviation:** Calculate the population standard deviation for `[2, 4, 6, 8]`.

12. **Sample standard deviation:** Calculate sample standard deviation for `[2, 4, 6, 8]`.

13. **25th percentile:** Find the 25th percentile of `np.arange(1, 11)`.

14. **Quartiles:** Find the 25th, 50th, and 75th percentiles of `[4, 8, 12, 16, 20]`.

15. **Quantile:** Calculate the 0.8 quantile of `np.arange(1, 11)`.

16. **Column means:** For a 3 × 2 matrix, calculate the mean of each column.

17. **Row means:** For a 3 × 2 matrix, calculate the mean of each row.

18. **Column sums:** Calculate the sum of each column in `[[1, 2, 3], [4, 5, 6]]`.

19. **Keep dimensions:** Calculate column means with `keepdims=True` and report the output shape.

20. **Weighted average:** Calculate the weighted average of values `[70, 90]` with weights `[1, 3]`.

21. **Missing value mean:** Compare `np.mean` and `np.nanmean` for `[1.0, 2.0, np.nan, 5.0]`.

22. **Missing value median:** Calculate `np.nanmedian` for `[1.0, np.nan, 9.0, 3.0]`.

23. **NaN-aware extrema:** Find `nanmin` and `nanmax` for `[np.nan, 4, 2, 10]`.

24. **NaN-aware standard deviation:** Calculate `np.nanstd` for `[1.0, 2.0, np.nan, 3.0]`.

25. **Correlation coefficient:** Use `np.corrcoef` to calculate the correlation between `[1, 2, 3]` and `[2, 4, 6]`.

26. **Negative correlation:** Calculate correlation between `[1, 2, 3]` and `[6, 4, 2]`.

27. **Covariance:** Use `np.cov` on `[1, 2, 3]` and `[2, 4, 6]`.

28. **Boolean count:** Count how many values in `[2, 5, 8, 11, 14]` are greater than 7 using `np.sum` on a condition.

29. **Outlier effect:** Compare mean and median for `[20, 21, 22, 23, 200]`.

30. **Axis and shape:** For a matrix of shape `(4, 3)`, predict the shapes of `mean(axis=0)` and `mean(axis=1)`. Verify in NumPy.

31. **Normalize columns:** Subtract each column's mean from a 3 × 2 array using `keepdims=True`.

32. **Percentile of scores:** Find the 90th percentile of `[45, 55, 65, 75, 85, 95]`.

33. **Repeated extrema:** For `[4, 1, 7, 1, 7]`, show the results of `argmin` and `argmax`.

34. **Summarize a dataset:** For `[12, 15, 15, 18, 20, 25]`, report mean, median, standard deviation, and range.

35. **Mini challenge: class report:** For a 5 × 3 marks matrix (5 students, 3 subjects), calculate each subject's mean, median, and standard deviation.

