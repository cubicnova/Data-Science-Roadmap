# Solutions — Database Fundamentals

Suggested answers follow. SQL dialects vary slightly; test statements in your chosen database.

## 1. Exercise 1

A database stores organized data; a DBMS is the software that manages that data and provides access, integrity, and administration features.

## 2. Exercise 2

Table: structured collection of records; row: one record; column: attribute; schema: database structure; query: request to retrieve or change data.

## 3. Exercise 3

Relational: PostgreSQL, MySQL, SQLite, SQL Server. Non-relational examples: MongoDB (document), Redis (key-value).

## 4. Exercise 4

A relational database fits transactional student enrollment where entities have clear relationships and constraints matter.

## 5. Exercise 5

```sql
CREATE TABLE departments (
  department_id INTEGER PRIMARY KEY,
  department_name VARCHAR(100) NOT NULL UNIQUE
);
```

## 6. Exercise 6

```sql
CREATE TABLE students (
  student_id INTEGER PRIMARY KEY,
  student_name VARCHAR(100) NOT NULL,
  department_id INTEGER,
  FOREIGN KEY (department_id) REFERENCES departments(department_id)
);
```

## 7. Exercise 7

```sql
INSERT INTO departments (department_id, department_name) VALUES
(1, 'Data Science'), (2, 'Computer Science'), (3, 'Cybersecurity');
```

## 8. Exercise 8

```sql
INSERT INTO students (student_id, student_name, department_id) VALUES
(101, 'Asha', 1), (102, 'Ravi', 2), (103, 'Neha', 1),
(104, 'Kabir', 3), (105, 'Mina', 2);
```

## 9. Exercise 9

```sql
SELECT student_id, student_name FROM students;
```

## 10. Exercise 10

```sql
SELECT student_id, student_name
FROM students
WHERE department_id = 2;
```

## 11. Exercise 11

A primary key identifies a row unambiguously. Duplicate values would identify more than one row, while NULL would mean the row lacks its required identifier.

## 12. Exercise 12

Use a composite primary key `(student_id, course_id)` if each student can enroll in a course only once; alternatively use a surrogate enrollment ID plus a unique constraint on that pair.

## 13. Exercise 13

A primary key identifies a row in its own table. A foreign key refers to a key in another (or the same) table to enforce valid relationships.

## 14. Exercise 14

```sql
ALTER TABLE courses ADD COLUMN credits INTEGER;
```

## 15. Exercise 15

```sql
CREATE TABLE products (
  product_id INTEGER PRIMARY KEY,
  product_name VARCHAR(100) NOT NULL,
  price DECIMAL(10,2) NOT NULL CHECK (price >= 0)
);
```

## 16. Exercise 16

`NOT NULL` requires a value; `UNIQUE` prevents duplicate values; `DEFAULT` supplies a value when omitted; `CHECK` validates a condition.

## 17. Exercise 17

```sql
UPDATE courses
SET course_name = 'SQL for Analytics'
WHERE course_id = 2;
```

## 18. Exercise 18

```sql
DELETE FROM students
WHERE student_id = 105;
``` 

The `WHERE` clause limits the deletion to the intended row; without it, every row may be deleted.

## 19. Exercise 19

One-to-one: one profile per user. One-to-many: one department has many students. Many-to-many: many students take many courses.

## 20. Exercise 20

```sql
CREATE TABLE enrollments (
  student_id INTEGER NOT NULL,
  course_id INTEGER NOT NULL,
  enrolled_on DATE,
  PRIMARY KEY (student_id, course_id),
  FOREIGN KEY (student_id) REFERENCES students(student_id),
  FOREIGN KEY (course_id) REFERENCES courses(course_id)
);
```

## 21. Exercise 21

1NF requires each field to hold one value at the defined row granularity. Instead of storing `course_ids = '1,2,3'` in one student cell, store separate enrollment rows.

## 22. Exercise 22

If a department phone number is repeated on every student row, changing it requires many updates; some rows may remain stale. Store department details once in a departments table.

## 23. Exercise 23

A transaction is a logical unit of work that commits together or rolls back. A bank transfer subtracts from one account and adds to another in one transaction.

## 24. Exercise 24

Atomicity: all-or-nothing; Consistency: valid rules remain true; Isolation: concurrent work is controlled; Durability: committed changes persist.

## 25. Exercise 25

```sql
CREATE INDEX idx_students_department
ON students(department_id);
```

## 26. Exercise 26

Benefit: faster filtering or joins on the indexed column. Cost: extra storage and slower writes because the index must be maintained.

## 27. Exercise 27

Parameterized queries keep data values separate from SQL code, reducing SQL injection risk and handling quoting and types more safely.

## 28. Exercise 28

`DELETE` removes rows; `DROP TABLE` removes the table definition and its data; `TRUNCATE` empties a table efficiently where supported. Transaction and identity-reset behavior varies by DBMS.

## 29. Exercise 29

```sql
SELECT department_id, COUNT(*) AS student_count
FROM students
GROUP BY department_id;
```

## 30. Exercise 30

```sql
SELECT s.student_name, d.department_name
FROM students AS s
JOIN departments AS d
  ON s.department_id = d.department_id;
```

## 31. Exercise 31

Referential integrity prevents a foreign key from referencing a parent key that does not exist, subject to configured constraint rules.

## 32. Exercise 32

Age: INTEGER; price: DECIMAL/NUMERIC; date of birth: DATE; name: VARCHAR or TEXT. Exact choices depend on DBMS and requirements.

## 33. Exercise 33

Review requirements, back up, create a versioned migration, test on representative data, assess locking/downtime, deploy in a controlled window, verify results, and prepare a rollback plan.

## 34. Exercise 34

Use `books(book_id, title, author_id, ...)`, `members(member_id, name, ...)`, `authors(author_id, name)`, and `loans(loan_id, book_id, member_id, loan_date, due_date, returned_at)`. Foreign keys link loans to one book and one member; add constraints to prevent invalid loan states.

## 35. Exercise 35

```python
import sqlite3
con = sqlite3.connect(":memory:")
con.execute("PRAGMA foreign_keys = ON")
con.executescript("""
CREATE TABLE departments (
  department_id INTEGER PRIMARY KEY,
  department_name TEXT NOT NULL UNIQUE
);
CREATE TABLE students (
  student_id INTEGER PRIMARY KEY,
  student_name TEXT NOT NULL,
  department_id INTEGER,
  FOREIGN KEY (department_id) REFERENCES departments(department_id)
);
CREATE TABLE courses (
  course_id INTEGER PRIMARY KEY,
  course_name TEXT NOT NULL
);
CREATE TABLE enrollments (
  student_id INTEGER,
  course_id INTEGER,
  PRIMARY KEY (student_id, course_id),
  FOREIGN KEY (student_id) REFERENCES students(student_id),
  FOREIGN KEY (course_id) REFERENCES courses(course_id)
);
INSERT INTO departments VALUES (1, 'Data Science'), (2, 'Computer Science');
INSERT INTO students VALUES (101, 'Asha', 1), (102, 'Ravi', 2);
INSERT INTO courses VALUES (10, 'SQL Basics'), (11, 'Statistics');
INSERT INTO enrollments VALUES (101, 10), (101, 11), (102, 10);
""")
rows = con.execute("""
SELECT s.student_name, c.course_name
FROM enrollments e
JOIN students s ON s.student_id = e.student_id
JOIN courses c ON c.course_id = e.course_id
ORDER BY s.student_name, c.course_name
""").fetchall()
print(rows)
con.close()
```
