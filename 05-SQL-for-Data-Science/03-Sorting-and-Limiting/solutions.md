# Solutions — Sorting and Limiting in SQL

Suggested solutions use SQLite/PostgreSQL-style `LIMIT`; adjust syntax for your DBMS.

## 1. Exercise 1

```sql
SELECT * FROM products ORDER BY price ASC;
```

## 2. Exercise 2

```sql
SELECT * FROM products ORDER BY price DESC;
```

## 3. Exercise 3

```sql
SELECT * FROM students ORDER BY student_name ASC;
```

## 4. Exercise 4

```sql
SELECT * FROM students ORDER BY department ASC, age DESC;
```

## 5. Exercise 5

```sql
SELECT * FROM orders ORDER BY order_date DESC, order_id DESC;
```

## 6. Exercise 6

```sql
SELECT * FROM orders ORDER BY order_date ASC, total DESC, order_id ASC;
```

## 7. Exercise 7

```sql
SELECT product_name, price FROM products ORDER BY price DESC, product_id ASC LIMIT 5;
```

## 8. Exercise 8

```sql
SELECT product_name, price FROM products ORDER BY price ASC, product_id ASC LIMIT 3;
```

## 9. Exercise 9

```sql
SELECT * FROM orders ORDER BY order_date DESC, order_id DESC LIMIT 10;
```

## 10. Exercise 10

```sql
SELECT * FROM products ORDER BY product_id LIMIT 10 OFFSET 10;
```

## 11. Exercise 11

Without `ORDER BY`, SQL does not promise a specific result order, so the subset selected by `LIMIT` may vary with plans, indexes, or data changes.

## 12. Exercise 12

```sql
SELECT product_name, price, price * 0.90 AS discounted_price
FROM products
ORDER BY discounted_price ASC, product_id ASC;
```

## 13. Exercise 13

```sql
SELECT product_name FROM products ORDER BY LENGTH(product_name) ASC, product_name ASC;
```
The string-length function may differ by database (for example, `LEN` in SQL Server).

## 14. Exercise 14

```sql
SELECT student_name, score FROM students
ORDER BY CASE WHEN score IS NULL THEN 1 ELSE 0 END, score DESC, student_name ASC;
```

## 15. Exercise 15

```sql
SELECT student_name, email FROM students
ORDER BY CASE WHEN email IS NULL THEN 1 ELSE 0 END, student_name ASC;
```

## 16. Exercise 16

```sql
SELECT TOP (5) product_name, price
FROM products
ORDER BY price DESC, product_id ASC;
```

## 17. Exercise 17

```sql
SELECT student_id, student_name
FROM students
ORDER BY student_name, student_id
FETCH FIRST 10 ROWS ONLY;
```
Check that your database version supports this syntax.

## 18. Exercise 18

Ascending goes from lower to higher values (or A to Z); descending goes from higher to lower (or Z to A).

## 19. Exercise 19

```sql
SELECT student_name, score FROM students ORDER BY score DESC, student_name ASC;
```

## 20. Exercise 20

```sql
SELECT customer_id, SUM(total) AS spending
FROM orders
GROUP BY customer_id
ORDER BY spending DESC, customer_id ASC
LIMIT 10;
```

## 21. Exercise 21

```sql
SELECT student_id, student_name, age
FROM students
ORDER BY age DESC, student_id ASC
LIMIT 5;
```

## 22. Exercise 22

```sql
SELECT product_id, product_name
FROM products
WHERE product_id > 500
ORDER BY product_id
LIMIT 10;
```

## 23. Exercise 23

Rows may be inserted or removed before the offset, shifting page boundaries. Large offsets can also require the database to scan and discard many rows.

## 24. Exercise 24

An index whose key order matches the requested sort can let the database read rows in that order without a separate sort, depending on the query and plan.

## 25. Exercise 25

```sql
SELECT student_id, student_name, score
FROM students
ORDER BY score DESC, student_id ASC;
```
This gives a sorted list but does not isolate one row per rank. Use `ROW_NUMBER()` for exactly one row per group or `RANK()`/`DENSE_RANK()` to preserve ties.

## 26. Exercise 26

`RANK()` and `DENSE_RANK()` preserve tied ranks; `LIMIT` returns a fixed number of rows and can cut off ties at the boundary.

## 27. Exercise 27

```sql
SELECT * FROM orders ORDER BY order_date DESC;
```
This assumes ISO-format text `YYYY-MM-DD` (or consistent sortable ISO timestamps), for which lexical order matches chronological order.

## 28. Exercise 28

```sql
WITH ranked AS (
  SELECT o.*, ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date DESC, order_id DESC) AS rn
  FROM orders AS o
)
SELECT * FROM ranked WHERE rn = 1;
```

## 29. Exercise 29

```sql
SELECT * FROM products ORDER BY category ASC, price DESC, product_id ASC;
```

## 30. Exercise 30

```sql
SELECT * FROM products
ORDER BY CASE WHEN price IS NULL THEN 1 ELSE 0 END, price ASC, product_id ASC;
```

## 31. Exercise 31

```sql
SELECT * FROM products ORDER BY RANDOM() LIMIT 5;
```

## 32. Exercise 32

Storage layout and query plans can change, and parallel execution or indexes may alter the returned order. Only `ORDER BY` defines the required order.

## 33. Exercise 33

```sql
SELECT product_id, product_name, price
FROM products
ORDER BY price DESC, product_id ASC
LIMIT 5;
```
The unique product ID makes ties deterministic.

## 34. Exercise 34

```sql
SELECT * FROM products ORDER BY product_id LIMIT 10 OFFSET 20;
```
This skips the first 20 rows in product-ID order and returns the next 10.

## 35. Exercise 35

```python
import sqlite3
import pandas as pd
con = sqlite3.connect(":memory:")
con.executescript("""
CREATE TABLE products (product_id INTEGER PRIMARY KEY, product_name TEXT, category TEXT, price REAL);
INSERT INTO products VALUES
(1, 'Mouse', 'Accessories', 20),
(2, 'Laptop', 'Computers', 900),
(3, 'Keyboard', 'Accessories', 40),
(4, 'Tablet', 'Computers', 300),
(5, 'Cable', 'Accessories', 10),
(6, 'Monitor', 'Computers', 300);
""")
print(pd.read_sql_query("SELECT * FROM products ORDER BY price DESC, product_id ASC LIMIT 3", con))
print(pd.read_sql_query("SELECT * FROM products ORDER BY product_id LIMIT 2 OFFSET 2", con))
con.close()
```
