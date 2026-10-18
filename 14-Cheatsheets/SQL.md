# SQL Cheat Sheet

A practical, beginner-friendly reference for querying relational databases using SQL (Structured Query Language). This cheat sheet covers database fundamentals, CRUD operations, filtering, joins, aggregation, subqueries, Common Table Expressions (CTEs), window functions, database design, and SQL techniques for data analysis and machine learning.

**Repository:** [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap)

---

## Table of Contents

1. [Introduction to SQL](#1-introduction-to-sql)
2. [SQL Categories](#2-sql-categories)
3. [Database and Table Fundamentals](#3-database-and-table-fundamentals)
4. [Creating Databases and Tables](#4-creating-databases-and-tables)
5. [Inserting Data](#5-inserting-data)
6. [SELECT Queries](#6-select-queries)
7. [Filtering Data](#7-filtering-data)
8. [Sorting and Limiting Results](#8-sorting-and-limiting-results)
9. [SQL Operators](#9-sql-operators)
10. [SQL Functions](#10-sql-functions)
11. [Aggregate Functions and GROUP BY](#11-aggregate-functions-and-group-by)
12. [SQL Joins](#12-sql-joins)
13. [Subqueries](#13-subqueries)
14. [Common Table Expressions (CTEs)](#14-common-table-expressions-ctes)
15. [Window Functions](#15-window-functions)
16. [CASE Expressions and Conditional Logic](#16-case-expressions-and-conditional-logic)
17. [Updating and Deleting Data](#17-updating-and-deleting-data)
18. [Constraints and Keys](#18-constraints-and-keys)
19. [Indexes and Query Performance](#19-indexes-and-query-performance)
20. [Transactions](#20-transactions)
21. [Views and Temporary Tables](#21-views-and-temporary-tables)
22. [Database Design and Normalization](#22-database-design-and-normalization)
23. [Date and Time Operations](#23-date-and-time-operations)
24. [SQL for Data Analysis](#24-sql-for-data-analysis)
25. [SQL Security Best Practices](#25-sql-security-best-practices)
26. [Common Errors and Solutions](#26-common-errors-and-solutions)
27. [Practice Exercises](#27-practice-exercises)
28. [Quick Revision](#28-quick-revision)
29. [Official Resources](#29-official-resources)

---

## 1. Introduction to SQL

SQL stands for Structured Query Language. It is used to create, retrieve, modify, and manage data in relational database management systems (RDBMS).

### Common use cases

- Retrieving data from relational databases.
- Filtering and aggregating large datasets.
- Combining data from multiple tables.
- Generating business reports and dashboards.
- Preparing datasets for data science and machine learning.
- Managing database schemas and relationships.
- Supporting analytics, reporting, and data engineering pipelines.

### Popular SQL databases

| Database | Common use case |
|---|---|
| PostgreSQL | Applications, analytics, and advanced SQL |
| MySQL | Web applications and transactional systems |
| SQLite | Lightweight, embedded databases |
| Microsoft SQL Server | Enterprise applications and business intelligence |
| Oracle Database | Enterprise and large-scale business systems |
| DuckDB | Local analytics and data science workflows |

### Basic SQL query

```sql
SELECT employee_name, salary
FROM employees
WHERE salary > 50000;
```

This query retrieves the names and salaries of employees whose salaries exceed 50,000.

---

## 2. SQL Categories

SQL commands are commonly organized into five categories.

| Category | Full form | Purpose | Examples |
|---|---|---|---|
| DDL | Data Definition Language | Defines database structures | `CREATE`, `ALTER`, `DROP` |
| DML | Data Manipulation Language | Modifies table data | `INSERT`, `UPDATE`, `DELETE` |
| DQL | Data Query Language | Retrieves data | `SELECT` |
| DCL | Data Control Language | Manages permissions | `GRANT`, `REVOKE` |
| TCL | Transaction Control Language | Controls transactions | `COMMIT`, `ROLLBACK` |

The classification of `SELECT` as DQL is common in educational materials; some references group it under DML.

### DDL example

```sql
CREATE TABLE employees (
    employee_id INTEGER PRIMARY KEY,
    employee_name VARCHAR(100),
    salary DECIMAL(10, 2)
);
```

### DML example

```sql
INSERT INTO employees (
    employee_id,
    employee_name,
    salary
)
VALUES (1, 'Amit', 55000);
```

### DQL example

```sql
SELECT *
FROM employees;
```

### DCL example

```sql
GRANT SELECT ON employees TO analyst;
```

Permission syntax and supported roles vary by database.

### TCL example

```sql
BEGIN;

UPDATE employees
SET salary = 60000
WHERE employee_id = 1;

COMMIT;
```

---

## 3. Database and Table Fundamentals

A relational database organizes data into tables containing rows and columns.

Consider this sample `employees` table:

| employee_id | employee_name | department | salary |
|---:|---|---|---:|
| 1 | Amit | IT | 55000 |
| 2 | Priya | HR | 60000 |
| 3 | Rahul | IT | 65000 |
| 4 | Neha | Finance | 70000 |
| 5 | Karan | HR | 50000 |

### Important terminology

- **Database:** An organized collection of data.
- **Table:** A collection of related records.
- **Row:** A single record.
- **Column:** An attribute of a record.
- **Primary key:** Uniquely identifies each row.
- **Foreign key:** References a key in another table.
- **Schema:** A namespace or logical organization for database objects; its exact meaning varies by database.
- **Query:** An instruction sent to a database.

### Example relational structure

<CodeBlock language="sql">CREATE TABLE departments (
    department_id INTEGER PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL
);

CREATE TABLE employees (
    employee_id INTEGER PRIMARY KEY,
    employee_name VARCHAR(100) NOT NULL,
    department_id INTEGER,
    salary DECIMAL(10, 2),
    FOREIGN KEY (department_id)
        REFERENCES departments(department_id)
);</CodeBlock>

The foreign key links each employee to a department.

---

## 4. Creating Databases and Tables

### 4.1 Create a database

```sql
CREATE DATABASE company_db;
```

Database creation syntax and privileges vary by database engine.

### 4.2 Create a table

```sql
CREATE TABLE employees (
    employee_id INTEGER PRIMARY KEY,
    employee_name VARCHAR(100) NOT NULL,
    department VARCHAR(50),
    age INTEGER,
    salary DECIMAL(10, 2),
    joining_date DATE
);
```

### 4.3 Common SQL data types

| Data type | Purpose | Example |
|---|---|---|
| `INTEGER` | Whole numbers | `25` |
| `BIGINT` | Large whole numbers | `9000000000` |
| `DECIMAL(p,s)` | Exact decimal values | `55000.50` |
| `FLOAT` / `REAL` | Approximate numeric values | `3.14159` |
| `CHAR(n)` | Fixed-length text | `"IN"` |
| `VARCHAR(n)` | Variable-length text | `"Amit"` |
| `TEXT` | Longer text | Description |
| `DATE` | Calendar date | `'2026-01-15'` |
| `TIME` | Time of day | `'10:30:00'` |
| `TIMESTAMP` | Date and time | `'2026-01-15 10:30:00'` |
| `BOOLEAN` | True or false | `TRUE` |
| `BLOB` / binary types | Binary data | File contents |

Data type names and supported features differ across database engines.

### 4.4 Add a column

```sql
ALTER TABLE employees
ADD email VARCHAR(150);
```

### 4.5 Rename a column

PostgreSQL example:

```sql
ALTER TABLE employees
RENAME COLUMN employee_name TO full_name;
```

Other databases may use different syntax.

### 4.6 Remove a column

```sql
ALTER TABLE employees
DROP COLUMN email;
```

This is a schema change and may permanently remove stored data.

### 4.7 Remove a table

```sql
DROP TABLE employees;
```

**Warning:** `DROP TABLE` removes the table structure and its data. Verify the target before executing it.

---

## 5. Inserting Data

### 5.1 Insert a single row

```sql
INSERT INTO employees (
    employee_id,
    employee_name,
    department,
    age,
    salary,
    joining_date
)
VALUES (
    1,
    'Amit',
    'IT',
    25,
    55000,
    '2026-01-10'
);
```

### 5.2 Insert multiple rows

```sql
INSERT INTO employees (
    employee_id,
    employee_name,
    department,
    age,
    salary
)
VALUES
    (2, 'Priya', 'HR', 30, 60000),
    (3, 'Rahul', 'IT', 28, 65000),
    (4, 'Neha', 'Finance', 35, 70000);
```

### 5.3 Insert default values

```sql
INSERT INTO employees (
    employee_id,
    employee_name
)
VALUES (5, 'Karan');
```

Columns not specified may receive their defined default values or `NULL`, if allowed.

### 5.4 Insert from another query

```sql
INSERT INTO high_salary_employees (
    employee_id,
    employee_name,
    salary
)
SELECT
    employee_id,
    employee_name,
    salary
FROM employees
WHERE salary >= 60000;
```

The destination table must already exist and have compatible columns.

### 5.5 Retrieve inserted rows

```sql
SELECT *
FROM employees;
```

Some database engines support `RETURNING` or `OUTPUT` clauses to return inserted rows directly.

---

## 6. SELECT Queries

### 6.1 Select all columns

```sql
SELECT *
FROM employees;
```

Use `*` for quick inspection. In production queries, explicitly listing required columns is generally clearer and more robust.

### 6.2 Select specific columns

```sql
SELECT employee_name, salary
FROM employees;
```

### 6.3 Use column aliases

```sql
SELECT
    employee_name AS name,
    salary AS monthly_salary
FROM employees;
```

### 6.4 Calculate values

```sql
SELECT
    employee_name,
    salary,
    salary * 12 AS annual_salary
FROM employees;
```

This assumes salary is a monthly amount.

### 6.5 Remove duplicate results

```sql
SELECT DISTINCT department
FROM employees;
```

### 6.6 Select a limited number of rows

PostgreSQL, MySQL, and SQLite:

```sql
SELECT *
FROM employees
LIMIT 5;
```

SQL Server commonly uses:

```sql
SELECT TOP 5 *
FROM employees;
```

### 6.7 Use expressions in SELECT

```sql
SELECT
    employee_name,
    salary,
    salary * 0.10 AS estimated_bonus
FROM employees;
```

### 6.8 Use a table alias

```sql
SELECT e.employee_name, e.salary
FROM employees AS e;
```

Aliases help improve readability, especially when queries use multiple tables.

---

## 7. Filtering Data

The `WHERE` clause filters rows based on a condition.

### 7.1 Comparison operators

```sql
SELECT *
FROM employees
WHERE salary > 50000;
```

| Operator | Meaning |
|---|---|
| `=` | Equal to |
| `<>` | Not equal to |
| `!=` | Not equal to; supported by many databases |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal to |
| `<=` | Less than or equal to |

### 7.2 AND condition

```sql
SELECT *
FROM employees
WHERE department = 'IT'
  AND salary > 50000;
```

### 7.3 OR condition

```sql
SELECT *
FROM employees
WHERE department = 'IT'
   OR department = 'HR';
```

### 7.4 BETWEEN

```sql
SELECT *
FROM employees
WHERE salary BETWEEN 50000 AND 65000;
```

`BETWEEN` includes both endpoints.

### 7.5 IN

```sql
SELECT *
FROM employees
WHERE department IN ('IT', 'HR');
```

### 7.6 NOT IN

```sql
SELECT *
FROM employees
WHERE department NOT IN ('HR', 'Finance');
```

Be careful if a `NOT IN` subquery can return `NULL`, as this can produce unexpected filtering behavior. `NOT EXISTS` is often safer for anti-join logic.

### 7.7 LIKE pattern matching

```sql
SELECT *
FROM employees
WHERE employee_name LIKE 'A%';
```

Common patterns:

| Pattern | Meaning |
|---|---|
| `'A%'` | Starts with A |
| `'%a'` | Ends with a |
| `'%mit%'` | Contains mit |
| `'_m%'` | Second character is m |

`%` matches zero or more characters, while `_` matches one character. Case sensitivity depends on the database and collation.

### 7.8 IS NULL

```sql
SELECT *
FROM employees
WHERE department IS NULL;
```

Do not use `department = NULL` to check for missing values.

### 7.9 IS NOT NULL

```sql
SELECT *
FROM employees
WHERE department IS NOT NULL;
```

### 7.10 Combine conditions

```sql
SELECT *
FROM employees
WHERE (department = 'IT' OR department = 'HR')
  AND salary >= 55000;
```

Use parentheses to make logical precedence explicit.

---

## 8. Sorting and Limiting Results

### 8.1 Sort ascending

```sql
SELECT *
FROM employees
ORDER BY salary ASC;
```

### 8.2 Sort descending

```sql
SELECT *
FROM employees
ORDER BY salary DESC;
```

### 8.3 Sort by multiple columns

```sql
SELECT *
FROM employees
ORDER BY department ASC, salary DESC;
```

### 8.4 Return the top five salaries

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 5;
```

Use a unique tie-breaker if you need deterministic ordering when salaries are equal.

### 8.5 Pagination with LIMIT and OFFSET

```sql
SELECT *
FROM employees
ORDER BY employee_id
LIMIT 10 OFFSET 20;
```

This returns up to 10 rows after skipping the first 20.

### 8.6 SQL Server pagination

```sql
SELECT *
FROM employees
ORDER BY employee_id
OFFSET 20 ROWS
FETCH NEXT 10 ROWS ONLY;
```

An `ORDER BY` clause is important for consistent pagination.

---

## 9. SQL Operators

### 9.1 Arithmetic operators

```sql
SELECT
    salary,
    salary + 5000 AS increased_salary,
    salary - 2000 AS reduced_salary,
    salary * 12 AS annual_salary,
    salary / 12 AS monthly_fraction
FROM employees;
```

Common operators: `+`, `-`, `*`, `/`.

### 9.2 Logical operators

```sql
SELECT *
FROM employees
WHERE salary >= 50000
  AND age >= 25;
```

Common logical operators include `AND`, `OR`, and `NOT`.

### 9.3 Concatenate strings

PostgreSQL:

```sql
SELECT employee_name || ' - ' || department AS employee_info
FROM employees;
```

MySQL:

```sql
SELECT CONCAT(employee_name, ' - ', department) AS employee_info
FROM employees;
```

Concatenation syntax varies by database.

### 9.4 EXISTS

```sql
SELECT d.department_name
FROM departments AS d
WHERE EXISTS (
    SELECT 1
    FROM employees AS e
    WHERE e.department_id = d.department_id
);
```

`EXISTS` checks whether the subquery returns at least one row.

### 9.5 Set operators

```sql
SELECT employee_name
FROM current_employees

UNION

SELECT employee_name
FROM former_employees;
```

| Operator | Purpose |
|---|---|
| `UNION` | Combine results and remove duplicate rows |
| `UNION ALL` | Combine results while preserving duplicates |
| `INTERSECT` | Return rows common to both results |
| `EXCEPT` | Return rows in the first result but not the second |

`INTERSECT` and `EXCEPT` are not supported in exactly the same form by every database engine. Corresponding queries generally need compatible column counts and data types.

---

## 10. SQL Functions

### 10.1 Aggregate functions

```sql
SELECT
    COUNT(*) AS total_employees,
    AVG(salary) AS average_salary,
    MIN(salary) AS minimum_salary,
    MAX(salary) AS maximum_salary,
    SUM(salary) AS total_salary
FROM employees;
```

### 10.2 String functions

```sql
SELECT
    UPPER(employee_name) AS uppercase_name,
    LOWER(department) AS lowercase_department,
    LENGTH(employee_name) AS name_length
FROM employees;
```

String-length functions differ across engines. For example, SQL Server commonly uses `LEN()` instead of `LENGTH()`.

### 10.3 Numeric functions

```sql
SELECT
    ROUND(salary, 2) AS rounded_salary,
    ABS(salary - 60000) AS salary_difference
FROM employees;
```

Other common functions include `CEIL` or `CEILING`, `FLOOR`, and `POWER`, with syntax varying by engine.

### 10.4 NULL-handling functions

```sql
SELECT
    employee_name,
    COALESCE(department, 'Unassigned') AS department
FROM employees;
```

`COALESCE()` returns the first non-NULL expression.

MySQL also provides `IFNULL()`, while SQL Server provides `ISNULL()` with different type-handling behavior.

### 10.5 NULLIF

```sql
SELECT
    revenue / NULLIF(quantity, 0) AS unit_price
FROM sales;
```

`NULLIF(quantity, 0)` returns `NULL` when quantity is zero, helping avoid division-by-zero errors. Choose an appropriate result type and handle the resulting `NULL` as needed.

---

## 11. Aggregate Functions and GROUP BY

Aggregate functions summarize multiple rows into a single result or a result per group.

### 11.1 Count rows

```sql
SELECT COUNT(*) AS total_employees
FROM employees;
```

### 11.2 Count non-NULL values

```sql
SELECT COUNT(department) AS employees_with_department
FROM employees;
```

`COUNT(column)` excludes NULL values, whereas `COUNT(*)` counts rows.

### 11.3 Calculate average salary

```sql
SELECT AVG(salary) AS average_salary
FROM employees;
```

Most SQL aggregate functions ignore NULL inputs; `COUNT(*)` is the key exception.

### 11.4 Group by department

```sql
SELECT
    department,
    COUNT(*) AS employee_count,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department;
```

### 11.5 Group by multiple columns

```sql
SELECT
    department,
    city,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department, city;
```

### 11.6 Filter groups using HAVING

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 55000;
```

- `WHERE` filters rows before grouping.
- `HAVING` filters groups after aggregation.

### 11.7 Count distinct values

```sql
SELECT COUNT(DISTINCT department) AS department_count
FROM employees;
```

### 11.8 Conditional aggregation

```sql
SELECT
    department,
    COUNT(*) AS total_employees,
    SUM(CASE WHEN salary >= 60000 THEN 1 ELSE 0 END)
        AS high_salary_employees
FROM employees
GROUP BY department;
```

### 11.9 Calculate percentage of total

```sql
SELECT
    department,
    SUM(salary) AS department_salary,
    100.0 * SUM(salary) /
        NULLIF(
            (SELECT SUM(salary) FROM employees),
            0
        ) AS percentage_of_total
FROM employees
GROUP BY department;
```

The `100.0` encourages fractional arithmetic, although exact numeric type behavior varies across databases.

---

## 12. SQL Joins

Joins combine related rows from multiple tables.

Assume two tables:

**employees**

| employee_id | employee_name | department_id |
|---:|---|---:|
| 1 | Amit | 10 |
| 2 | Priya | 20 |
| 3 | Rahul | 10 |
| 4 | Neha | NULL |

**departments**

| department_id | department_name |
|---:|---|
| 10 | IT |
| 20 | HR |
| 30 | Finance |

### 12.1 INNER JOIN

Returns rows that match in both tables.

```sql
SELECT
    e.employee_name,
    d.department_name
FROM employees AS e
INNER JOIN departments AS d
    ON e.department_id = d.department_id;
```

### 12.2 LEFT JOIN

Returns all rows from the left table and matching rows from the right table. Unmatched right-side columns are NULL.

```sql
SELECT
    e.employee_name,
    d.department_name
FROM employees AS e
LEFT JOIN departments AS d
    ON e.department_id = d.department_id;
```

### 12.3 RIGHT JOIN

Returns all rows from the right table and matching rows from the left table.

```sql
SELECT
    e.employee_name,
    d.department_name
FROM employees AS e
RIGHT JOIN departments AS d
    ON e.department_id = d.department_id;
```

### 12.4 FULL OUTER JOIN

Returns matching rows and unmatched rows from both tables.

```sql
SELECT
    e.employee_name,
    d.department_name
FROM employees AS e
FULL OUTER JOIN departments AS d
    ON e.department_id = d.department_id;
```

MySQL does not directly support `FULL OUTER JOIN`. It can be emulated with carefully constructed `LEFT JOIN` and `RIGHT JOIN` queries using `UNION` or `UNION ALL` and appropriate duplicate handling.

### 12.5 CROSS JOIN

Returns every combination of rows from both tables.

```sql
SELECT
    e.employee_name,
    d.department_name
FROM employees AS e
CROSS JOIN departments AS d;
```

If one table contains 4 rows and another contains 3 rows, the result has 12 combinations.

### 12.6 SELF JOIN

Joins a table to itself.

```sql
SELECT
    e.employee_name AS employee,
    m.employee_name AS manager
FROM employees AS e
LEFT JOIN employees AS m
    ON e.manager_id = m.employee_id

