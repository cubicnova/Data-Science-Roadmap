# SQL Date and String Functions — 35 Practice Exercises

Use common SQL syntax and state your database dialect when a function is vendor-specific. Assume tables such as `customers`, `products`, and `orders` with the column names referenced in each question.

1. Explain the difference between a `DATE` and a `TIMESTAMP` data type.
2. Write a query that converts a city name to uppercase.
3. Write a query that converts email addresses to lowercase.
4. Write a query that removes leading and trailing spaces from customer names.
5. Explain the difference between `TRIM`, `LTRIM`, and `RTRIM`.
6. Write a query that returns the length of each product name. Mention one dialect difference.
7. Write a query to extract the first five characters of a product name.
8. Write a query that replaces spaces in a city name with underscores.
9. Write a query that concatenates a customer's name and email into one label.
10. Write a query to extract the part of an email address before `@`. Assume valid email values.
11. Use `LIKE` to find emails ending in `example.com` without case sensitivity where possible.
12. Explain the meaning of `%` and `_` in a `LIKE` pattern.
13. Write a query that groups customers by trimmed, lowercase city name and counts them.
14. Why can concatenating a `NULL` value produce an unexpected result?
15. Show how `COALESCE` can provide a fallback for a missing city.
16. Write a query that extracts the year and month from `order_date` using standard-style `EXTRACT` syntax.
17. Write a query that filters orders for January 2026 when `order_timestamp` includes time.
18. Why is a half-open date range often preferable to using `BETWEEN` with an end date?
19. Write a query that returns the current date and current timestamp.
20. Write a PostgreSQL-style expression that adds seven days to `order_date`.
21. Name two alternative SQL functions used to add dates in other database engines.
22. Write a query to calculate days between `order_date` and `shipped_date` in PostgreSQL.
23. Explain why `DATEDIFF` syntax and results should be checked for each database.
24. Write a PostgreSQL expression that formats an order date as `YYYY-MM`.
25. Give the MySQL equivalent for formatting a date as year-month.
26. Write a PostgreSQL query that counts orders by month.
27. Explain what `DATE_TRUNC('month', timestamp_column)` does.
28. How would you turn an empty string into `NULL` in SQL?
29. Why should date strings from external files be parsed with an explicit format?
30. Describe two risks of ignoring time zones when analyzing timestamps.
31. Write a query to normalize a customer's name by trimming spaces and converting it to lowercase.
32. Why might applying a function to a date column in `WHERE` reduce index efficiency?
33. Write a query to find orders shipped more than 10 days after ordering, using PostgreSQL-style date subtraction.
34. List four common data-quality problems handled by string functions.
35. List five best practices for working with date and string functions in SQL.
