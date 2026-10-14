# Practice Exercises — Sorting and Ranking in Pandas

**Instructions:** Use Pandas and small self-created DataFrames. Unless a question specifies otherwise, preserve the original DataFrame and show the output of your operation.

1. Create a DataFrame with names and scores for at least five students.
2. Sort the student DataFrame by score in ascending order.
3. Sort the student DataFrame by score in descending order.
4. Sort employees by department alphabetically.
5. Sort by department ascending and salary descending.
6. Sort a DataFrame by two numeric columns, with the first descending and the second ascending.
7. Sort names without case affecting alphabetical order using the `key` parameter.
8. Create a DataFrame containing missing scores and sort with missing scores first.
9. Sort the same DataFrame with missing scores last.
10. Sort a DataFrame by its index labels.
11. Sort DataFrame columns alphabetically using `sort_index(axis=1)`.
12. Sort values descending and reset the index without keeping the old index.
13. Use `rank()` to rank a Series from smallest to largest.
14. Rank a Series so the largest value receives rank 1.
15. Show the default average ranking for tied values.
16. Use `method='min'` to rank tied values.
17. Use `method='max'` to rank tied values.
18. Use `method='first'` and explain why row order matters.
19. Use `method='dense'` and compare it with `method='min'`.
20. Rank scores within each department, highest score first.
21. Return the three rows with the largest score using `nlargest()`.
22. Return the two rows with the smallest salary using `nsmallest()`.
23. Find the top three rows by score, breaking ties with experience ascending.
24. Sort a product table by category and then price ascending.
25. Create an ordered categorical with Low, Medium, High and sort it.
26. Sort dates chronologically after parsing a date column as datetime.
27. Sort transactions by date ascending and amount descending.
28. Rank sales within each region and show the best-ranked seller per region.
29. Find the highest score in each department after sorting or grouping.
30. Sort by a column while keeping the original DataFrame unchanged.
31. Use `inplace=True` to sort a DataFrame and inspect the changed order.
32. Compare `sort_values()` and `sort_index()` with a small example.
33. Demonstrate how `head(3)` depends on the current order.
34. Sort a DataFrame with two keys and use `ignore_index=True`.
35. Build a final report sorted by department, then descending score, with a dense rank column.
