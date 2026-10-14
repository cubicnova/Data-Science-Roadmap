# Practice Exercises — Data Cleaning with Pandas

Attempt each exercise with a small reproducible DataFrame. Keep raw data unchanged and document assumptions.

1. Load a CSV file and display the first 10 rows, shape, and data types.
2. Create a compact audit table with column names, data types, missing counts, and missing percentages.
3. Count fully duplicated rows in a DataFrame.
4. Standardize column names by trimming spaces, lowercasing, and replacing spaces with underscores.
5. Find column names that become duplicates after standardization.
6. Trim whitespace from every text column without modifying numeric columns.
7. Normalize an email column by trimming whitespace and converting values to lowercase.
8. Standardize status labels such as `Complete`, `complete`, and `DONE` to `completed`.
9. Convert a mixed numeric column to numbers and count values that failed conversion.
10. Convert date strings to datetime and report invalid dates.
11. Remove currency symbols and thousands separators from an amount column, then parse it numerically.
12. Report missing-value counts and percentages for every column.
13. Fill missing city values with `Unknown`.
14. Fill missing numeric ages with the median and preserve a flag for originally missing ages.
15. Drop rows missing a required customer ID and report how many rows were removed.
16. Remove exact duplicate rows and compare row counts before and after.
17. Keep the latest record per email after sorting by an update timestamp.
18. Identify rows with ages outside a plausible 0–120 range.
19. Flag malformed email-like values using a regular expression.
20. Find negative transaction amounts and summarize their count.
21. Detect outliers in an amount column using the IQR rule without deleting them.
22. Replace inconsistent representations of missing text such as `N/A`, `na`, and empty strings with missing values.
23. Convert a quantity column to nullable integer dtype after parsing.
24. Normalize phone numbers by removing spaces, parentheses, and hyphens.
25. Check that each order's shipped date is not earlier than its order date.
26. Identify records with a missing name but a present email.
27. Create a quality flag for rows with any missing values.
28. Produce a before-and-after report for row count, duplicate count, and missing cells.
29. Export a cleaned DataFrame to CSV without writing the index.
30. Write assertions that verify no duplicate customer IDs remain.
31. Split a full-name field into first name and remaining name, preserving missing values.
32. Standardize country names using a mapping dictionary.
33. Clean a dataset by applying column-name, text, type, missing-value, and duplicate rules in a documented order.
34. Create a function that returns a cleaned DataFrame and a dictionary of cleaning metrics.
35. Build an end-to-end cleaning pipeline for a small messy customer/order dataset and explain every rule.
