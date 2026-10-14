# Practice Exercises — DateTime and Text in Pandas

Try each task before consulting `solutions.md`. Use small DataFrames and verify your results.

1. Create a DataFrame with five date strings and parse the column using `pd.to_datetime()`.
2. Parse dates written as `DD/MM/YYYY` by supplying the correct `format`.
3. Convert an invalid date string to `NaT` without stopping execution.
4. Count how many values in a parsed datetime column are missing.
5. Extract year, month, and day into three separate columns.
6. Add a weekday-name column to a DataFrame.
7. Create a Boolean column that identifies month-end dates.
8. Filter rows on or after 1 January 2026.
9. Filter timestamp rows for January 2026 using a half-open interval.
10. Calculate the number of days between order and delivery dates.
11. Generate a daily date range containing 10 dates.
12. Generate timestamps every six hours for two days.
13. Extract the hour from a datetime column.
14. Convert UTC timestamps to the `Asia/Kolkata` timezone.
15. Create a column showing the quarter for each date.
16. Trim leading and trailing spaces from a customer-name column.
17. Normalize names to title case after trimming spaces.
18. Convert a text column to lowercase.
19. Find rows whose product name contains `pro`, ignoring case.
20. Find rows whose email ends with `example.com`.
21. Replace literal hyphens with spaces in a product-code column.
22. Replace all digit sequences with `#` using a regular expression.
23. Split a `city,state` column into two columns.
24. Extract the username and domain from email-like strings using named regex groups.
25. Create a text-length column for each description.
26. Check whether each code starts with `INV-`.
27. Use a regex full match to identify codes made of exactly three letters followed by four digits.
28. Replace multiple consecutive whitespace characters with a single space.
29. Remove a leading `ID:` prefix from strings when it exists.
30. Create a column containing the first five characters of each product code.
31. Find missing-safe matches for a text filter using `na=False`.
32. Combine date parsing and customer-name cleanup in one workflow.
33. Create a month-start column from a datetime Series.
34. Sort a DataFrame chronologically after parsing its date column.
35. Build a cleaned dataset that parses dates, standardizes names, extracts ticket types, and reports invalid dates.
