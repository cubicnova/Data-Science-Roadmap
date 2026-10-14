# 02 — SELECT and Filtering in SQL

Learn to retrieve exactly the data you need with `SELECT`, `WHERE`, sorting, limits, aliases, expressions, and SQL filtering operators. Examples use broadly portable SQL and SQLite where a runnable demonstration is useful.

## Learning objectives

- Select specific columns and all columns when appropriate.
- Rename result columns with aliases.
- Filter rows with comparison operators and logical conditions.
- Work safely with `NULL`, `IN`, `BETWEEN`, `LIKE`, and pattern matching.
- Sort results and limit returned rows.
- Use `DISTINCT`, expressions, and computed columns.
- Understand operator precedence and parameterized queries.

## 1. Basic SELECT

```sql
SELECT student_id, student_name
FROM students;
```

Use explicit column names for clarity. `SELECT *` is useful for exploration but can be less clear or efficient in production queries.

## 2. Aliases and expressions

```sql
SELECT
    student_name AS name,
    credits * 10 AS estimated_points
FROM students;
```

Aliases make output easier to read. Many databases support `AS` for column aliases; table alias syntax is widely supported too.

## 3. WHERE and comparison operators

```sql
SELECT product_name, price
FROM products
WHERE price >= 100;
```

Common operators: `=`, `<>` (not equal), `<`, `<=`, `>`, `>=`.

Strings are usually quoted with single quotes:
```sql
SELECT *
FROM students
WHERE department = 'Data Science';
```

## 4. AND, OR, NOT, and parentheses

```sql
SELECT *
FROM products
WHERE category = 'Laptop'
  AND price < 1000;
```

Use parentheses to make mixed logic explicit:
```sql
WHERE (category = 'Laptop' OR category = 'Tablet')
  AND price < 1000
```

`AND` is evaluated before `OR` in standard SQL, but explicit parentheses are easier to review.

## 5. IN and BETWEEN

```sql
SELECT *
FROM students
WHERE department IN ('Data Science', 'Computer Science');

SELECT *
FROM products
WHERE price BETWEEN 100 AND 500;
```

`BETWEEN` includes both endpoints. Use `NOT IN` carefully if the list or subquery may contain `NULL`, because SQL uses three-valued logic.

## 6. LIKE and wildcards

- `%` matches zero or more characters.
- `_` matches exactly one character.

```sql
SELECT *
FROM customers
WHERE customer_name LIKE 'A%';

SELECT *
FROM products
WHERE product_code LIKE 'AB_7';
```

Case sensitivity varies by database and collation. SQLite's default `LIKE` behavior is often case-insensitive for ASCII letters; other systems differ.

## 7. NULL checks

`NULL` means missing or unknown, not zero or an empty string. Use:
```sql
SELECT *
FROM students
WHERE email IS NULL;

SELECT *
FROM students
WHERE email IS NOT NULL;
```

Do not use `email = NULL`; comparisons with `NULL` evaluate to unknown. Use `COALESCE(email, 'Not provided')` to display a fallback value in many SQL systems.

## 8. DISTINCT, ORDER BY, and LIMIT

```sql
SELECT DISTINCT department
FROM students
ORDER BY department ASC;
```

```sql
SELECT product_name, price
FROM products
ORDER BY price DESC, product_name ASC
LIMIT 5;
```

`LIMIT` is supported by SQLite, PostgreSQL, and MySQL. SQL Server commonly uses `TOP` or `OFFSET ... FETCH`; Oracle has its own supported row-limiting syntax depending on version. Always include a meaningful `ORDER BY` when the selected rows must be predictable.

## 9. Filtering dates

```sql
SELECT *
FROM orders
WHERE order_date >= '2026-01-01'
  AND order_date < '2026-02-01';
```

A half-open interval is reliable for timestamps because it includes all times on the first date and excludes the next month. Date literal syntax differs across DBMSs.

## 10. Safe parameterized queries

In application code, pass user-provided values as parameters instead of concatenating them into SQL. Python SQLite example:

```python
cursor.execute(
    "SELECT student_id, student_name FROM students WHERE department_id = ?",
    (department_id,)
)
```

Placeholders differ by driver (for example, `?` or `%s`). Parameterization helps separate SQL code from values.

## 11. Sample practice schema

```sql
CREATE TABLE students (
    student_id INTEGER PRIMARY KEY,
    student_name TEXT NOT NULL,
    department TEXT,
    age INTEGER,
    email TEXT
);

CREATE TABLE products (
    product_id INTEGER PRIMARY KEY,
    product_name TEXT NOT NULL,
    category TEXT,
    price NUMERIC,
    product_code TEXT
);

CREATE TABLE orders (
    order_id INTEGER PRIMARY KEY,
    customer_id INTEGER,
    order_date TEXT,
    status TEXT,
    total NUMERIC
);
```

## 12. Query-writing checklist

1. Select only required columns.
2. Check whether filters should include boundary values.
3. Group mixed `AND`/`OR` conditions with parentheses.
4. Handle `NULL` with `IS NULL` or `IS NOT NULL`.
5. Use `ORDER BY` when order matters.
6. Verify the SQL dialect for `LIMIT`, date handling, and string matching.
7. Parameterize user-supplied values in applications.
8. Test filters on known sample records before using them for reporting.

Practice in `practice.md`, review `solutions.md`, and execute the SQLite examples in `examples.ipynb`.
