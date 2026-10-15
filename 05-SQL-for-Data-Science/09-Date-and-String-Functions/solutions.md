# SQL Date and String Functions — Solutions

These are reference answers. Function names and syntax differ among PostgreSQL, MySQL, SQL Server, SQLite, and other SQL engines; adapt where noted.

## 1. Solution

`DATE` stores a calendar date; `TIMESTAMP` stores a date and time, sometimes with time-zone semantics depending on the type and engine.
## 2. Solution

```sql
SELECT UPPER(city) AS city_upper FROM customers;
```
## 3. Solution

```sql
SELECT LOWER(email) AS normalized_email FROM customers;
```
## 4. Solution

```sql
SELECT TRIM(full_name) AS clean_name FROM customers;
```
## 5. Solution

`TRIM` removes spaces from both ends, `LTRIM` from the left, and `RTRIM` from the right. Optional character-set behavior varies by engine.
## 6. Solution

```sql
SELECT product_name, LENGTH(product_name) AS name_length FROM products;
```
SQL Server commonly uses `LEN`; byte/character length semantics can differ by engine and data type.
## 7. Solution

```sql
SELECT SUBSTRING(product_name FROM 1 FOR 5) AS first_five FROM products;
```
Some dialects use `SUBSTRING(product_name, 1, 5)` or `SUBSTR`.
## 8. Solution

```sql
SELECT REPLACE(city, ' ', '_') AS city_slug FROM customers;
```
## 9. Solution

```sql
SELECT CONCAT(full_name, ' <', email, '>') AS contact_label FROM customers;
```
If `CONCAT` is unavailable, use the dialect's concatenation operator. NULL behavior differs.
## 10. Solution

PostgreSQL-style: `SELECT SUBSTRING(email FROM 1 FOR POSITION('@' IN email) - 1) AS email_user FROM customers WHERE email LIKE '%@%';` Other engines may use `SUBSTRING_INDEX`, `SUBSTR`, `CHARINDEX`, or `INSTR`.
## 11. Solution

```sql
SELECT * FROM customers WHERE LOWER(email) LIKE '%@example.com';
```
The exact pattern may depend on whether the domain must match exactly and on collation rules.
## 12. Solution

`%` matches any sequence of zero or more characters; `_` matches exactly one character.
## 13. Solution

```sql
SELECT LOWER(TRIM(city)) AS normalized_city, COUNT(*) AS customer_count
FROM customers
GROUP BY LOWER(TRIM(city));
```
## 14. Solution

Some engines propagate `NULL` through concatenation; others' `CONCAT` functions may treat NULL differently. Check the engine and use `COALESCE` when needed.
## 15. Solution

```sql
SELECT COALESCE(city, 'Unknown') AS city FROM customers;
```
## 16. Solution

```sql
SELECT EXTRACT(YEAR FROM order_date) AS order_year,
       EXTRACT(MONTH FROM order_date) AS order_month
FROM orders;
```
## 17. Solution

```sql
SELECT * FROM orders
WHERE order_timestamp >= '2026-01-01'
  AND order_timestamp <  '2026-02-01';
```
## 18. Solution

A half-open range includes the start and excludes the next period's start, so it includes all timestamp precision within January without inventing an end-of-day time.
## 19. Solution

```sql
SELECT CURRENT_DATE, CURRENT_TIMESTAMP;
```
## 20. Solution

```sql
SELECT order_date + INTERVAL '7 days' AS one_week_later FROM orders;
```
## 21. Solution

Examples include MySQL `DATE_ADD(date, INTERVAL 7 DAY)` and SQL Server `DATEADD(day, 7, date)`.
## 22. Solution

```sql
SELECT shipped_date - order_date AS days_to_ship
FROM orders WHERE shipped_date IS NOT NULL;
```
## 23. Solution

Different engines may use different argument order, units, and counting rules (for example, elapsed duration versus date boundaries crossed).
## 24. Solution

```sql
SELECT TO_CHAR(order_date, 'YYYY-MM') AS order_month FROM orders;
```
## 25. Solution

```sql
SELECT DATE_FORMAT(order_date, '%Y-%m') AS order_month FROM orders;
```
## 26. Solution

```sql
SELECT DATE_TRUNC('month', order_timestamp) AS order_month,
       COUNT(*) AS order_count
FROM orders
GROUP BY DATE_TRUNC('month', order_timestamp)
ORDER BY order_month;
```
## 27. Solution

It returns a timestamp aligned to the beginning of the specified month, with smaller time units set to their start values; exact return type depends on the engine.
## 28. Solution

```sql
SELECT NULLIF(TRIM(value), '') AS value_or_null FROM source_table;
```
## 29. Solution

Explicit parsing formats reduce ambiguity such as whether `03/04/2026` means March 4 or April 3, and help detect malformed values consistently.
## 30. Solution

Examples: events can be assigned to the wrong reporting day; daylight-saving transitions can create repeated or missing local times; UTC/local conversion mistakes can shift daily or monthly aggregates.
## 31. Solution

```sql
SELECT LOWER(TRIM(full_name)) AS normalized_name FROM customers;
```
## 32. Solution

Wrapping an indexed column in a function may prevent a normal index seek or range scan. Prefer direct range predicates when possible, or use a supported expression index.
## 33. Solution

```sql
SELECT order_id, shipped_date - order_date AS days_to_ship
FROM orders
WHERE shipped_date IS NOT NULL
  AND shipped_date - order_date > 10;
```
## 34. Solution

Examples: leading/trailing spaces, inconsistent letter case, mixed separators, invalid dates, empty strings, malformed email addresses, and inconsistent text encodings.
## 35. Solution

Use native date/time types; prefer half-open timestamp ranges; normalize text consistently; handle NULLs and empty strings; parse with explicit formats; verify dialect-specific syntax; define time zones; preserve raw source data.
