# Solutions — SELECT and Filtering in SQL

These are suggested answers. SQL dialects differ in row-limiting syntax and string matching behavior.

## 1. Exercise 1

```sql
SELECT * FROM students;
```

## 2. Exercise 2

```sql
SELECT student_id, student_name FROM students;
```

## 3. Exercise 3

```sql
SELECT student_name AS name FROM students;
```

## 4. Exercise 4

```sql
SELECT product_name, price, price * 0.90 AS discounted_price FROM products;
```

## 5. Exercise 5

```sql
SELECT * FROM products WHERE price > 500;
```

## 6. Exercise 6

```sql
SELECT * FROM students WHERE age = 20;
```

## 7. Exercise 7

```sql
SELECT * FROM orders WHERE total <= 1000;
```

## 8. Exercise 8

```sql
SELECT * FROM students WHERE department <> 'Data Science';
```
This condition excludes `NULL` departments as well. If you want to include them, add `OR department IS NULL`.

## 9. Exercise 9

```sql
SELECT * FROM products WHERE price BETWEEN 100 AND 500;
```

## 10. Exercise 10

```sql
SELECT * FROM students WHERE department IN ('Data Science', 'Cybersecurity');
```

## 11. Exercise 11

```sql
SELECT * FROM customers WHERE customer_name LIKE 'A%';
```

## 12. Exercise 12

```sql
SELECT * FROM products WHERE product_name LIKE '%phone%';
```

## 13. Exercise 13

```sql
SELECT * FROM products WHERE product_code LIKE 'AB_7';
```
`_` matches one character.

## 14. Exercise 14

```sql
SELECT * FROM students WHERE email IS NULL;
```

## 15. Exercise 15

```sql
SELECT * FROM students WHERE email IS NOT NULL;
```

## 16. Exercise 16

```sql
SELECT DISTINCT department FROM students;
```

## 17. Exercise 17

```sql
SELECT product_name, price FROM products ORDER BY price DESC;
```

## 18. Exercise 18

```sql
SELECT student_id, student_name, department FROM students ORDER BY department ASC, student_name ASC;
```

## 19. Exercise 19

```sql
SELECT product_name, price FROM products ORDER BY price DESC LIMIT 5;
```
Use the row-limiting syntax supported by your DBMS.

## 20. Exercise 20

```sql
SELECT * FROM orders WHERE order_date >= '2026-01-01' AND order_date < '2026-02-01';
```

## 21. Exercise 21

```sql
SELECT * FROM products WHERE category <> 'Accessories';
```
This excludes rows with `category IS NULL`. To include unknown categories, use `WHERE category <> 'Accessories' OR category IS NULL`.

## 22. Exercise 22

```sql
SELECT * FROM students WHERE age BETWEEN 18 AND 25 AND department = 'Computer Science';
```

## 23. Exercise 23

```sql
SELECT * FROM products WHERE category IN ('Laptop', 'Tablet') AND price < 1000;
```

## 24. Exercise 24

```sql
SELECT * FROM orders WHERE status IN ('pending', 'processing') AND total > 200;
```
`IN` is concise here; parentheses and explicit `OR` are also valid.

## 25. Exercise 25

```sql
SELECT student_name, COALESCE(email, 'Not provided') AS email_display FROM students;
```

## 26. Exercise 26

```sql
SELECT * FROM students WHERE student_name LIKE '____';
```
In SQL `LIKE`, each underscore matches one character.

## 27. Exercise 27

```sql
SELECT * FROM students WHERE student_name LIKE '%a';
```

## 28. Exercise 28

`= NULL` is not a valid missing-value test because NULL comparisons evaluate to unknown. Use `IS NULL`.

## 29. Exercise 29

```sql
SELECT DISTINCT status FROM orders ORDER BY status ASC;
```

## 30. Exercise 30

```sql
SELECT * FROM products WHERE price > 0 AND price IS NOT NULL;
```
The `price > 0` predicate already excludes NULL under SQL logic; the explicit check may still communicate intent.

## 31. Exercise 31

```sql
SELECT * FROM orders ORDER BY order_date DESC, total DESC;
```

## 32. Exercise 32

```python
cursor.execute(
    "SELECT student_id, student_name FROM students WHERE department = ?",
    (department_value,)
)
rows = cursor.fetchall()
```

## 33. Exercise 33

String concatenation can allow input to alter the SQL statement's structure, leading to SQL injection. Use parameterized queries.

## 34. Exercise 34

```sql
SELECT product_name, price FROM products WHERE price BETWEEN 100 AND 500 ORDER BY price ASC LIMIT 10;
```

## 35. Exercise 35

```python
import sqlite3
con = sqlite3.connect(":memory:")
con.execute("CREATE TABLE students (student_id INTEGER, student_name TEXT, department TEXT, email TEXT)")
con.executemany("INSERT INTO students VALUES (?, ?, ?, ?)", [
    (1, "Asha", "Data Science", "asha@example.com"),
    (2, "Ravi", "Computer Science", None),
    (3, "Anaya", "Data Science", "anaya@example.com"),
])
print(con.execute("SELECT student_name FROM students WHERE department = ?", ("Data Science",)).fetchall())
print(con.execute("SELECT student_name FROM students WHERE student_name LIKE 'A%'").fetchall())
print(con.execute("SELECT student_name FROM students WHERE email IS NULL").fetchall())
print(con.execute("SELECT student_name FROM students ORDER BY student_name LIMIT 2").fetchall())
con.close()
```
