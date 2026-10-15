# SQL Date and String Functions

Date and string functions help transform text, extract meaningful parts of dates, format output, and calculate time intervals. Exact function names and date formats vary across SQL databases; this module uses broadly common patterns and highlights differences.

## Learning objectives

- Clean and transform text using case, trimming, replacement, and substring functions.
- Search and combine strings.
- Extract parts of dates and calculate intervals.
- Filter date ranges safely.
- Understand common SQL dialect differences.

## 1. String functions

Assume a table `customers(customer_id, full_name, email, city)`.

### Case conversion

```sql
SELECT UPPER(city) AS city_upper,
       LOWER(email) AS normalized_email
FROM customers;
```

### Remove leading/trailing spaces

```sql
SELECT TRIM(full_name) AS clean_name,
       LTRIM(city) AS no_leading_spaces,
       RTRIM(city) AS no_trailing_spaces
FROM customers;
```

`TRIM`, `LTRIM`, and `RTRIM` behavior and optional character arguments can differ by database.

### Length and substring

```sql
SELECT full_name,
       LENGTH(full_name) AS name_length,
       SUBSTRING(full_name FROM 1 FOR 5) AS first_five
FROM customers;
```

Some databases use `SUBSTRING(text, start, length)`; SQL Server commonly uses `LEN()` for length, while MySQL and PostgreSQL support `LENGTH()` with differing byte/character nuances.

### Concatenate strings

```sql
SELECT CONCAT(full_name, ' <', email, '>') AS contact_label
FROM customers;
```

`CONCAT` availability and NULL behavior differ by engine. In some systems `||` is the concatenation operator; SQL Server commonly uses `+` or `CONCAT`.

### Replace and locate

```sql
SELECT REPLACE(city, ' ', '_') AS city_slug
FROM customers;
```

Search functions differ: `POSITION`, `INSTR`, `CHARINDEX`, and `STRPOS` are common variants.

### Extract text

```sql
SELECT SUBSTRING(email FROM 1 FOR POSITION('@' IN email) - 1) AS email_user
FROM customers
WHERE email LIKE '%@%';
```

The exact syntax is dialect-specific. Validate malformed strings before calculating substring lengths.

## 2. Pattern matching and text cleanup

```sql
SELECT *
FROM customers
WHERE LOWER(email) LIKE '%@example.com';
```

- `%` matches zero or more characters.
- `_` matches one character in a `LIKE` pattern.
- `LIKE` case sensitivity depends on collation and database.
- Regular-expression functions vary widely across database engines.

Normalize before grouping when appropriate:

```sql
SELECT LOWER(TRIM(city)) AS normalized_city, COUNT(*) AS customer_count
FROM customers
GROUP BY LOWER(TRIM(city));
```

Avoid changing raw source data irreversibly; consider storing cleaned values in a separate field or transformation layer.

## 3. Date and time basics

Common date types include `DATE`, `TIME`, `TIMESTAMP` / `DATETIME`, and time-zone-aware timestamp types. Use real date/time types instead of strings whenever possible.

### Extract parts of a date

```sql
-- PostgreSQL-style
SELECT EXTRACT(YEAR FROM order_date) AS order_year,
       EXTRACT(MONTH FROM order_date) AS order_month
FROM orders;
```

Other dialects may use `YEAR(order_date)`, `MONTH(order_date)`, `DATE_PART`, or `DATEPART`.

### Filter a date range

Prefer a half-open range, especially for timestamp columns:

```sql
SELECT *
FROM orders
WHERE order_timestamp >= '2026-01-01'
  AND order_timestamp <  '2026-02-01';
```

This includes all timestamps in January without relying on the precision of an end-of-day value.

### Current date and timestamp

Common variants include `CURRENT_DATE`, `CURRENT_TIMESTAMP`, `NOW()`, and `GETDATE()`. Availability and time-zone behavior vary.

## 4. Date arithmetic

### Add or subtract an interval

PostgreSQL-style:

```sql
SELECT order_date + INTERVAL '7 days' AS one_week_later,
       order_date - INTERVAL '1 month' AS one_month_earlier
FROM orders;
```

Other databases use functions such as `DATE_ADD`, `DATE_SUB`, or `DATEADD`.

### Difference between dates

```sql
-- PostgreSQL: difference between DATE values returns an integer day count
SELECT shipped_date - order_date AS days_to_ship
FROM orders;
```

Other systems may use `DATEDIFF`, `TIMESTAMPDIFF`, or timestamp subtraction. Check argument order and whether the result is days, boundaries crossed, or a duration.

## 5. Formatting and truncation

Formatting functions are dialect-specific. Examples:
- PostgreSQL: `TO_CHAR(order_date, 'YYYY-MM')`
- MySQL: `DATE_FORMAT(order_date, '%Y-%m')`
- SQL Server: `FORMAT(order_date, 'yyyy-MM')` (often less efficient than simpler conversions)

Date truncation examples:
- PostgreSQL: `DATE_TRUNC('month', order_timestamp)`
- Other engines may use `DATETRUNC`, `DATE_FORMAT`, or casts.

Do not compare formatted date strings when a native date comparison is available.

## 6. NULLs, invalid data, and time zones

- `NULL` values propagate through many string expressions; use `COALESCE` when a default is appropriate.
- `NULLIF(value, '')` can turn empty strings into `NULL`.
- Parse external date strings with an explicit format and handle failures deliberately.
- Store timestamps with a clear time-zone policy, often UTC, and convert for display.
- Be careful with daylight-saving transitions and month-end arithmetic.
- Do not wrap an indexed date column in a function in a filter unless necessary; a direct range predicate is often more index-friendly.

## 7. Common analytical examples

### Monthly order counts (PostgreSQL-style)

```sql
SELECT DATE_TRUNC('month', order_timestamp) AS order_month,
       COUNT(*) AS order_count
FROM orders
GROUP BY DATE_TRUNC('month', order_timestamp)
ORDER BY order_month;
```

### Days between order and shipment

```sql
SELECT order_id, shipped_date - order_date AS days_to_ship
FROM orders
WHERE shipped_date IS NOT NULL;
```

### Normalize customer names

```sql
SELECT customer_id,
       INITCAP(LOWER(TRIM(full_name))) AS normalized_name
FROM customers;
```

`INITCAP` is not available in every database; use a suitable equivalent or handle capitalization in application code.

## 8. Best practices

1. Use typed dates/timestamps, not lexically sorted text dates.
2. Use half-open date ranges for timestamps.
3. Check your SQL dialect before copying formatting or date arithmetic syntax.
4. Handle `NULL`, empty strings, and malformed inputs explicitly.
5. Normalize case/whitespace consistently before matching or grouping.
6. Keep raw data when transformations must be auditable.
7. Use explicit formats when parsing external date strings.
8. Define the time zone for timestamps and reporting boundaries.

## Quick reference

| Task | Common function or pattern |
|---|---|
| Upper/lower case | `UPPER`, `LOWER` |
| Trim spaces | `TRIM`, `LTRIM`, `RTRIM` |
| Length | `LENGTH`, `LEN` |
| Substring | `SUBSTRING`, `SUBSTR` |
| Replace text | `REPLACE` |
| Concatenate | `CONCAT`, `||` |
| Pattern matching | `LIKE` |
| Extract date parts | `EXTRACT`, `YEAR`, `MONTH`, `DATEPART` |
| Current date/time | `CURRENT_DATE`, `CURRENT_TIMESTAMP`, `NOW` |
| Date addition | `INTERVAL`, `DATE_ADD`, `DATEADD` |
| Date difference | date subtraction, `DATEDIFF`, `TIMESTAMPDIFF` |
| Format date | `TO_CHAR`, `DATE_FORMAT`, `FORMAT` |
| Truncate date/time | `DATE_TRUNC`, `DATETRUNC` |

## Practice

Work through [`practice.md`](practice.md), compare your responses with [`solutions.md`](solutions.md), and run the examples in [`examples.ipynb`](examples.ipynb).
