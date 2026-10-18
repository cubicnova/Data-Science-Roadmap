# Pandas and NumPy Interview Practice

Attempt the questions before checking `solution.md`. Unless stated otherwise, use the sample data created in `examples.ipynb`. Explain your reasoning, including expected shapes, index behavior, missing-value handling, and join cardinality.

## Section A — NumPy

1. Create a 1-D array containing integers from 0 through 11 and reshape it into a 3-by-4 array.
2. From a 2-D array, select the second row and the last two columns.
3. Given a 2-D array, calculate the mean of each column and the sum of each row.
4. Use a Boolean mask to select values greater than 10.
5. Explain broadcasting and predict the shape of adding arrays with shapes `(3, 1)` and `(1, 4)`.
6. Normalize a numeric array using min-max scaling. What should you consider if every value is identical?
7. Explain the difference between `arr.sort()` and `np.sort(arr)`.
8. Why is vectorized NumPy arithmetic generally preferable to iterating over every element in Python?

## Section B — Pandas selection and cleaning

9. Select the `name` and `score` columns from a DataFrame.
10. Filter rows where score is at least 80 and department is either `Data` or `AI`.
11. Return the first three rows by integer position, regardless of index labels.
12. Find rows with missing scores and fill missing scores with the column median.
13. Remove exact duplicate rows. Then explain why duplicate business keys may require a different rule.
14. Convert a text date column to datetime and identify invalid values safely.
15. Standardize category labels by trimming whitespace and converting to lowercase.
16. Explain why `df[df["score"] > 0]["flag"] = True` is unsafe and rewrite it.

## Section C — Grouping and transformation

17. Calculate mean score and row count per department.
18. Return the top-scoring row in each department. Define a deterministic tie-breaker.
19. Add a `department_mean` column to every row without reducing the DataFrame's row count.
20. Calculate each row's score as a percentage of its department's total score. Handle zero totals if needed.
21. Explain the difference between `agg`, `transform`, and `apply`.

## Section D — Merge, concatenate, and reshape

22. Merge employee data with department metadata using a department key.
23. Find employee records that have no matching department record.
24. Explain how duplicate keys on both sides of a merge can create a many-to-many result.
25. Combine two same-schema DataFrames by stacking rows. When would you use `ignore_index=True`?
26. Convert a long table of date, product, and sales into a wide summary using `pivot_table`.
27. Convert wide monthly columns into a long format using `melt`.

## Section E — Dates and performance

28. Extract year and month from a datetime column.
29. Compute a 3-observation rolling mean for a time-ordered series.
30. Explain the difference between a row-based rolling window and a time-based rolling window.
31. List three practical ways to reduce Pandas memory usage.
32. A merge unexpectedly doubles the row count. What do you inspect first?
33. A calculation is slow because it uses `DataFrame.apply(axis=1)`. How would you investigate and potentially improve it?

## Section F — Concepts and edge cases

34. What is the difference between a NumPy array and a Pandas DataFrame?
35. Explain `loc` versus `iloc`, and `at` versus `iat`.
36. What do `axis=0` and `axis=1` mean for a 2-D NumPy aggregation?
37. What are `NaN`, `None`, and `pd.NA`? Why should missing-value checks use `isna()`?
38. Explain a view versus a copy at a practical level.
39. What does `merge(validate="one_to_one")` check?
40. When might `category` dtype reduce memory usage, and when might it not help?

## Bonus challenge — Sales quality report

Using a DataFrame with `order_id`, `order_date`, `region`, `product`, `quantity`, and `unit_price`:
- Create `revenue = quantity * unit_price`.
- Report total revenue and order count by region.
- Identify duplicate `order_id` values.
- Report missing values by column.
- Build a product-by-month revenue table.
- State how you would validate that the report has not double-counted orders.

## Mock interview checklist

For each solution, be prepared to discuss:
- Input and output shapes.
- Index alignment and dtype changes.
- Missing values and duplicate keys.
- Time and memory complexity at a high level.
- Why your chosen API is safer or clearer than an alternative.
