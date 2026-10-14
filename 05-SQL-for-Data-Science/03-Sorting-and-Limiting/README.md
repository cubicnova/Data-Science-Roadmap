# 03 — Sorting and Limiting in SQL

Learn how to control the order and number of rows returned by SQL queries. This module covers `ORDER BY`, ascending and descending order, multi-column sorting, NULL ordering, row limiting, pagination, and deterministic results.

## Learning objectives

- Sort query results with `ORDER BY`.
- Use `ASC` and `DESC`.
- Sort by multiple columns with tie-breakers.
- Sort using expressions and aliases where supported.
- Understand how `NULL` values may sort differently across databases.
- Limit results with `LIMIT`, `TOP`, or `FETCH FIRST`.
- Use offset-based pagination and keyset pagination.
- Write deterministic queries for top-N analysis.

## 1. Basic ORDER BY

```sql
SELECT product_name, price
FROM products
ORDER BY price ASC;
```

`ASC` is ascending and is the default in most SQL systems. `DESC` is descending:

```sql
SELECT product_name, price
FROM products
ORDER BY price DESC;
```

SQL result order is not guaranteed unless the query includes `ORDER BY`.

## 2. Sort by multiple columns

```sql
SELECT department, student_name, age
FROM students
ORDER BY department ASC, age DESC, student_name ASC;
```

The database sorts by the first expression, then uses the next expression to break ties. Include a unique key as the final sort column when you need a stable and predictable order.

## 3. Sort by expressions

```sql
SELECT product_name, price, price * 0.9 AS sale_price
FROM products
ORDER BY sale_price ASC;
```

Most common SQL engines support ordering by a selected alias, but exact rules vary. You can repeat the expression in `ORDER BY` for broader portability.

## 4. Handling NULL values

`NULL` ordering differs by database and sort direction. Some systems support explicit `NULLS FIRST` or `NULLS LAST`:

```sql
SELECT student_name, score
FROM students
ORDER BY score DESC NULLS LAST;
```

For broader portability, use a `CASE` expression:

```sql
SELECT student_name, score
FROM students
ORDER BY
  CASE WHEN score IS NULL THEN 1 ELSE 0 END,
  score DESC,
  student_name ASC;
```

This places non-NULL scores first, highest to lowest, then sorts tied names alphabetically.

## 5. Limiting rows

SQLite, PostgreSQL, and MySQL commonly use `LIMIT`:

```sql
SELECT product_name, price
FROM products
ORDER BY price DESC, product_id ASC
LIMIT 5;
```

Other common dialects:
- SQL Server: `SELECT TOP (5) ... ORDER BY ...`
- Standard-style syntax supported by several systems: `FETCH FIRST 5 ROWS ONLY`
- Oracle supports row-limiting syntax in modern versions.

Use `ORDER BY` with a limit to make the selected rows predictable. Without an order, the database may return any qualifying rows.

## 6. OFFSET pagination

```sql
SELECT product_id, product_name
FROM products
ORDER BY product_id
LIMIT 10 OFFSET 20;
```

This skips 20 rows and returns the next 10 in SQLite/PostgreSQL/MySQL-style syntax. Large offsets can be expensive, and concurrent inserts/deletes can cause rows to shift between pages.

## 7. Keyset pagination

For large tables or changing datasets, keyset pagination can be more efficient and stable:

```sql
SELECT product_id, product_name
FROM products
WHERE product_id > :last_seen_id
ORDER BY product_id
LIMIT 10;
```

The placeholder syntax depends on the driver. For a composite sort order, the continuation condition must match the sort keys and direction. Keyset pagination works best with indexed, stable keys.

## 8. Top-N and bottom-N analysis

Top five most expensive products:
```sql
SELECT product_name, price
FROM products
ORDER BY price DESC, product_id ASC
LIMIT 5;
```

Bottom three prices:
```sql
SELECT product_name, price
FROM products
ORDER BY price ASC, product_id ASC
LIMIT 3;
```

If ties at the cutoff must all be included, use a ranking function such as `RANK()` or `DENSE_RANK()` rather than relying on a simple row limit.

## 9. Random ordering

Some engines support `ORDER BY RANDOM()` (SQLite/PostgreSQL) or `ORDER BY RAND()` (MySQL). Random sorting can be costly on large tables. Use it mainly for small samples or testing.

## 10. Best practices

1. Always specify `ORDER BY` when result order matters.
2. Add a unique tie-breaker to top-N queries.
3. Decide how NULLs should be handled.
4. Confirm the row-limit syntax for your DBMS.
5. Use indexed sort keys when appropriate and inspect query plans.
6. Prefer keyset pagination for large, frequently changing result sets.
7. Avoid relying on implicit row order or physical storage order.
8. When the business question concerns tied rankings, decide whether ties should be included.

Complete `practice.md`, run `examples.ipynb`, and review `solutions.md`.
