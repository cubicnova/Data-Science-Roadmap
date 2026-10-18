# Data Science Coding Challenges — Practice

Try each problem without looking at `solution.md`. For each task, write a function, add at least three tests, and state time and space complexity.

## Level 1 — Python fundamentals

1. Return the frequency of every item in a list.
2. Find the first non-repeating character in a string.
3. Remove duplicates from a list while preserving original order.
4. Check whether two strings are anagrams, ignoring case and spaces.
5. Return the top `k` most frequent values. Define tie behavior.
6. Flatten a one-level nested list without using external libraries.
7. Find the missing integer from a sequence containing distinct values in `0..n`.
8. Merge two sorted lists into one sorted list.

## Level 2 — Arrays and numerical operations

9. Compute the mean and population variance of a numeric list.
10. Normalize values with min-max scaling and handle a constant array.
11. Given an array, return indices of two values that sum to a target.
12. Clip all negative values to zero using NumPy.
13. Compute row means and column means for a 2-D array.
14. Explain and demonstrate broadcasting for shapes `(n, 1)` and `(1, m)`.
15. Calculate cosine similarity between two nonzero vectors; define behavior for zero vectors.
16. Detect outliers using the IQR rule and return the mask rather than deleting rows.

## Level 3 — Pandas and data wrangling

17. Fill missing numeric values with the median of each column.
18. Return the three highest-scoring rows per group, with deterministic tie-breaking.
19. Calculate a group mean and add it to each original row.
20. Merge two DataFrames and detect unmatched keys.
21. Convert a wide DataFrame to long format with `melt`.
22. Parse a date column, extract month, and calculate a rolling mean after sorting.
23. Identify duplicate customer IDs and keep the latest record by timestamp.
24. Build a data-quality report with missing count, missing percentage, and distinct count per column.

## Level 4 — Statistics and machine-learning metrics

25. Implement accuracy, precision, recall, and F1 for binary labels without scikit-learn.
26. Compute a confusion matrix for binary classification.
27. Calculate mean absolute error (MAE) and root mean squared error (RMSE).
28. Implement train/test splitting conceptually and explain why preprocessing must be fit only on training data.
29. Write a bootstrap function that estimates a confidence interval for the mean.
30. Given scores and labels, explain how changing a classification threshold affects precision and recall.
31. Explain why accuracy is misleading for a dataset with 99% negatives.
32. Detect target leakage in a proposed feature list.

## Level 5 — ML workflow and debugging

33. Build a scikit-learn pipeline for numeric and categorical features with missing values.
34. Compare a baseline classifier with logistic regression using stratified validation.
35. Explain how to select cross-validation for grouped or time-ordered data.
36. Diagnose a model whose training score is high but test score is low.
37. Write unit tests for a function that normalizes an array.
38. A Pandas merge doubles the row count. Write checks to locate the cause.
39. A metric suddenly changes after a data pipeline release. List the first five checks.
40. Refactor a long notebook cell into reusable functions with clear inputs and outputs.

## Bonus challenge — Feature summary function

Write `summarize_numeric(df)` that returns one row per numeric column with:
- Non-missing count
- Missing count
- Mean and standard deviation
- Minimum, median, and maximum
- Number of unique non-missing values

The function should handle an empty DataFrame and columns containing only missing values.

## Evaluation rubric

Score each solution from 0 to 2 on:
- Correctness
- Edge-case handling
- Readability
- Tests
- Complexity explanation

A strong solution scores 8–10 out of 10 and explains the trade-offs clearly.
