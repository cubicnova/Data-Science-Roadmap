# Practice Exercises — Duplicates and Data Types in Pandas

**Instructions:** Use Pandas and small example DataFrames. Preserve a raw copy and explain any assumptions about which duplicate record should be retained.

1. Create a DataFrame containing at least two exact duplicate rows.
2. Use duplicated() to mark duplicate rows.
3. Count the number of duplicate rows in a DataFrame.
4. Display every row that belongs to a duplicate group.
5. Detect duplicates based only on a customer_id column.
6. Drop exact duplicate rows while keeping the first occurrence.
7. Drop duplicate customer IDs while keeping the last occurrence.
8. Explain the difference between keep='first', keep='last', and keep=False.
9. Sort by an updated_at column and keep the latest record for each ID.
10. Find duplicated email addresses while ignoring other columns.
11. Count the frequency of each customer ID using value_counts().
12. Inspect a DataFrame's dtypes and info() output.
13. Convert a column of numeric strings to integers.
14. Convert a numeric string column containing an invalid word with errors='coerce'.
15. Identify which original values failed numeric conversion.
16. Convert a date string column into datetime values.
17. Convert a string column to Pandas' nullable string dtype.
18. Convert a numeric column to nullable Int64 while preserving missing values.
19. Map 'true' and 'false' strings to actual Boolean values.
20. Explain why astype(bool) can be unsafe for strings like 'False'.
21. Clean comma-separated thousands values such as '1,250' before numeric conversion.
22. Convert invalid date strings to NaT and count the invalid dates.
23. Convert a low-cardinality status column to category.
24. Check whether a customer_id column is unique after deduplication.
25. Create a mask that marks every duplicate record by a composite key of name and date.
26. Remove duplicates based on name and date while keeping the first record.
27. Show how duplicate rows can be legitimate repeated events rather than data errors.
28. Create a cleaning workflow that preserves a raw DataFrame copy.
29. Compare row count before and after dropping duplicates.
30. Use duplicated(keep=False) and sort the resulting rows by the duplicate key.
31. Convert a mixed string/number column to numeric and report missing values after conversion.
32. Convert a timestamp column to datetime and extract its year.
33. Validate a cleaned dataset by checking dtypes, null counts, and duplicate keys.
34. Write a reusable function that removes duplicates using a supplied subset and keep policy.
35. Build an end-to-end example that deduplicates customer records and fixes ID, age, date, and active dtypes.
