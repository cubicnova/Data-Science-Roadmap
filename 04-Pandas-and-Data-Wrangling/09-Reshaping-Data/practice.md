# Practice Exercises — Reshaping Data in Pandas

**Instructions:** Use small DataFrames and inspect the result after every transformation. Check duplicate keys before using pivot().

1. Create a wide DataFrame with student names and scores in two subjects.
2. Use melt() to convert the wide student DataFrame to long format.
3. Set custom var_name and value_name labels in melt().
4. Use value_vars to melt only selected measurement columns.
5. Keep an identifier column while melting three measurement columns.
6. Explain one situation where long format is more useful than wide format.
7. Use pivot() to convert a unique long-format dataset back to wide format.
8. Reset the index after pivoting so the identifier becomes a column.
9. Demonstrate why pivot() fails when index-column combinations are duplicated.
10. Use pivot_table() with aggfunc='mean' for duplicated combinations.
11. Use pivot_table() with aggfunc='sum' to summarize sales by region and product.
12. Fill missing pivot-table combinations with zero using fill_value.
13. Add row and column totals to a pivot table using margins=True.
14. Create a frequency table with crosstab() for region and product.
15. Normalize a crosstab by index to get row percentages.
16. Normalize a crosstab by columns to get column proportions.
17. Use crosstab() with values and aggfunc to summarize a numeric measure.
18. Use stack() to move columns into a row index.
19. Use unstack() to move an index level into columns.
20. Create a MultiIndex DataFrame and demonstrate unstack().
21. Check for duplicate key combinations before using pivot().
22. Reshape monthly sales columns into month and sales columns using melt().
23. Convert a long temperature dataset to wide format by city and day.
24. Create a pivot table of average scores by class and subject.
25. Compare pivot() and pivot_table() and state when each is appropriate.
26. Use crosstab() to count outcomes by department.
27. Create a normalized crosstab showing the overall proportion of each combination.
28. Reshape a DataFrame with id, height, and weight into long format.
29. Use pivot_table() to calculate multiple aggregation functions.
30. After reshaping, verify that the total of a numeric measure is preserved.
31. Use melt() with ignore_index=False and inspect the resulting index.
32. Handle missing combinations in a pivot table and explain the meaning of fill_value.
33. Create a report with regions as rows, products as columns, and total revenue as values.
34. Reshape a dataset and then use groupby() to calculate an aggregate.
35. Build an end-to-end example converting wide sales data to long, summarizing it, and pivoting it back to a report.
