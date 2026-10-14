# 01 — Database Fundamentals

An introduction to databases and relational database concepts for data science. This module uses standard SQL examples that can be adapted to PostgreSQL, MySQL, SQLite, or SQL Server with minor syntax differences.

## Learning objectives

- Explain what a database, DBMS, table, row, column, schema, and query are.
- Distinguish relational and non-relational databases.
- Identify primary keys, foreign keys, and candidate keys.
- Understand relationships, constraints, and normalization.
- Write basic SQL statements to create, inspect, and query tables.
- Understand transactions, indexes, and data integrity.

## 1. Core terminology

- **Database:** organized collection of related data.
- **DBMS:** software used to create, query, secure, and maintain databases.
- **Relational database:** stores data in tables with defined relationships.
- **Table:** rows (records) and columns (attributes).
- **Schema:** structure and definitions of database objects.
- **SQL:** language used to define and manipulate relational data.
- **Query:** request to retrieve or modify data.

Example table `students`:

| student_id | name | department |
|---:|---|---|
| 101 | Asha | Data Science |
| 102 | Ravi | Computer Science |

## 2. Relational versus non-relational databases

Relational systems such as PostgreSQL, MySQL, SQLite, and SQL Server organize data in related tables and commonly use SQL. Non-relational systems include document, key-value, graph, and wide-column databases. Choose a database based on query patterns, consistency needs, data shape, scale, operations, and team experience—not on a single “best” label.

## 3. Keys

- **Primary key:** uniquely identifies each row; cannot be `NULL`.
- **Foreign key:** references a key in another table and helps preserve referential integrity.
- **Candidate key:** any minimal attribute set that could uniquely identify a row.
- **Composite key:** key made from multiple columns.
- **Natural key:** meaningful business value, such as a verified product code.
- **Surrogate key:** generated identifier with no business meaning.

```sql
CREATE TABLE departments (
    department_id INTEGER PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL UNIQUE
);

CREATE TABLE students (
    student_id INTEGER PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    department_id INTEGER,
    FOREIGN KEY (department_id)
        REFERENCES departments(department_id)
);
```

## 4. Data types and constraints

Common SQL types include integer, decimal/numeric, character/string, date, timestamp, and Boolean (support varies by database). Constraints communicate rules:
- `NOT NULL`: value is required.
- `UNIQUE`: values must be unique under the database's comparison rules.
- `PRIMARY KEY`: unique, non-null row identifier.
- `FOREIGN KEY`: referenced value must exist, subject to constraint options.
- `CHECK`: value must satisfy a condition.
- `DEFAULT`: supplies a value when an insert omits the column.

```sql
CREATE TABLE products (
    product_id INTEGER PRIMARY KEY,
    product_name VARCHAR(120) NOT NULL,
    price DECIMAL(10, 2) NOT NULL CHECK (price >= 0),
    stock INTEGER NOT NULL DEFAULT 0 CHECK (stock >= 0)
);
```

## 5. Basic SQL statements

### Create and inspect
```sql
CREATE TABLE courses (
    course_id INTEGER PRIMARY KEY,
    course_name VARCHAR(100) NOT NULL
);
```

### Insert
```sql
INSERT INTO courses (course_id, course_name)
VALUES (1, 'SQL Fundamentals');
```

### Read
```sql
SELECT course_id, course_name
FROM courses;
```

### Update and delete
```sql
UPDATE courses
SET course_name = 'Database Fundamentals'
WHERE course_id = 1;

DELETE FROM courses
WHERE course_id = 1;
```

Always inspect the `WHERE` condition before running `UPDATE` or `DELETE`. Without it, every row may be affected.

## 6. Relationships

- **One-to-one:** each row in A relates to at most one row in B.
- **One-to-many:** one department can have many students.
- **Many-to-many:** students can enroll in many courses and each course can have many students. Model this with a junction table such as `enrollments(student_id, course_id)`.

## 7. Normalization overview

Normalization reduces unnecessary duplication and update anomalies:
- **1NF:** each field contains a single value at the chosen level of representation; rows are identifiable.
- **2NF:** in 1NF and every non-key attribute depends on the whole candidate key, not just part of a composite key.
- **3NF:** in 2NF and non-key attributes do not depend transitively on candidate keys.
- **Denormalization:** intentionally adds redundancy or precomputed data for performance or simpler reads; it requires consistency controls.

Normalize based on functional dependencies and application needs, not by blindly splitting every table.

## 8. Transactions and ACID

A transaction groups changes into a logical unit. ACID means:
- **Atomicity:** all changes in the transaction happen or none do.
- **Consistency:** constraints and rules remain satisfied.
- **Isolation:** concurrent transactions are controlled according to the isolation level.
- **Durability:** committed changes survive failures as promised by the system.

```sql
BEGIN;
UPDATE accounts SET balance = balance - 500 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 500 WHERE account_id = 2;
COMMIT;
```

Use `ROLLBACK` instead of `COMMIT` if the transaction should be abandoned. Exact transaction syntax and behavior vary by database.

## 9. Indexes

An index can accelerate lookups, joins, and sorting, but uses storage and adds work to inserts, updates, and deletes. Index columns that match real query patterns and verify performance with the database's query plan tools.

```sql
CREATE INDEX idx_students_department
ON students(department_id);
```

## 10. Database safety and good practice

1. Use parameterized queries in application code; never concatenate untrusted input into SQL.
2. Apply least-privilege access and separate application roles where appropriate.
3. Back up data and test restoration.
4. Use transactions for related changes.
5. Define constraints to protect data integrity.
6. Avoid `SELECT *` in production queries when explicit columns are clearer.
7. Be careful with destructive statements and test on non-production data.
8. Use migrations and version control for schema changes.
9. Know your SQL dialect: data types, auto-generated keys, date functions, and transaction behavior vary.
10. Treat sample credentials and personal data securely.

## 11. Sample schema for practice

```sql
CREATE TABLE departments (
    department_id INTEGER PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL UNIQUE
);

CREATE TABLE students (
    student_id INTEGER PRIMARY KEY,
    student_name VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE,
    department_id INTEGER,
    FOREIGN KEY (department_id) REFERENCES departments(department_id)
);

CREATE TABLE courses (
    course_id INTEGER PRIMARY KEY,
    course_name VARCHAR(100) NOT NULL,
    credits INTEGER NOT NULL CHECK (credits > 0)
);

CREATE TABLE enrollments (
    student_id INTEGER,
    course_id INTEGER,
    enrolled_on DATE,
    grade VARCHAR(2),
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students(student_id),
    FOREIGN KEY (course_id) REFERENCES courses(course_id)
);
```

See `practice.md` for exercises, `solutions.md` for suggested answers, and `examples.ipynb` for a runnable SQLite demonstration.
