# SQL Interview Practice — Suggested Solutions

These are reference answers, not the only valid answers. They use SQLite-compatible syntax where possible. Adjust date functions and limiting syntax for your interview's database engine.

## Assumed schema

The notebook creates:
- `employees(employee_id, name, department_id, manager_id, salary, hire_date)`
- `departments(department_id, department_name)`
- `orders(order_id, customer_id, order_date, amount, category)`

## Section A — Fundamentals

### 1. Employees by descending salary

```sql
SELECT name, department_id, salary
FROM employees
ORDER BY salary DESC, name;
```

### 2. Distinct department IDs

```sql
SELECT DISTINCT department_id
FROM employees
ORDER BY department_id;
```

`DISTINCT` treats NULL as one distinct output value in this context.

### 3. Salary at least 80,000

```sql
SELECT employee_id, name, salary
FROM employees
WHERE salary >= 80000;
```

### 4. Names beginning with A

```sql
SELECT employee_id, name
FROM employees
WHERE name LIKE 'A%';
```

Case sensitivity for `LIKE` depends on the database and collation.

### 5. Unknown department

```sql
SELECT employee_id, name
FROM employees
WHERE department_id IS NULL;
```

Use `IS NULL`, not `= NULL`.

## Section B — Aggregation

### 6. Employee count per department

```sql
SELECT department_id, COUNT(*) AS employee_count
FROM employees
GROUP BY department_id
ORDER BY department_id;
```

This includes a NULL department group if any employee has a NULL department ID.

### 7. Average salary per department

```sql
SELECT department_id, AVG(salary) AS average_salary
FROM employees
GROUP BY department_id
ORDER BY average_salary DESC;
```

`AVG` ignores NULL salary values.

### 8. Departments with at least two employees

```sql
SELECT department_id, COUNT(*) AS employee_count
FROM employees
GROUP BY department_id
HAVING COUNT(*) >= 2;
```

`HAVING` filters groups after aggregation.

### 9. Non-NULL salary count versus row count

```sql
SELECT COUNT(*) AS total_rows,
       COUNT(salary) AS non_null_salaries
FROM employees;
```

`COUNT(salary)` excludes NULLs.

### 10. Revenue by category

```sql
SELECT category, SUM(amount) AS total_revenue
FROM orders
GROUP BY category
ORDER BY total_revenue DESC;
```

Assumes each row represents one order-level amount. If orders have multiple line items, aggregate at the intended grain to avoid double counting.

## Section C — Joins

### 11. Employees with department names, preserving unmatched employees

```sql
SELECT e.name, d.department_name
FROM employees AS e
LEFT JOIN departments AS d
  ON e.department_id = d.department_id
ORDER BY e.employee_id;
```

### 12. Departments with no employees

```sql
SELECT d.department_id, d.department_name
FROM departments AS d
LEFT JOIN employees AS e
  ON e.department_id = d.department_id
WHERE e.employee_id IS NULL;
```

`NOT EXISTS` is another clear option:

```sql
SELECT d.department_id, d.department_name
FROM departments AS d
WHERE NOT EXISTS (
  SELECT 1
  FROM employees AS e
  WHERE e.department_id = d.department_id
);
```

### 13. Employee and manager names

```sql
SELECT e.name AS employee_name,
       m.name AS manager_name
FROM employees AS e
LEFT JOIN employees AS m
  ON e.manager_id = m.employee_id;
```

The left join keeps top-level employees who have no manager.

### 14. Why joins multiply rows

A one-to-many join produces one output row for each matching pair. If an order has four item rows, that order appears four times after joining to items. Aggregate at the intended grain, pre-aggregate the many-side, or count distinct entity IDs when appropriate.

## Section D — NULLs, CASE, and subqueries

### 15. Salary bands, including missing salary

```sql
SELECT name,
       CASE
         WHEN salary IS NULL THEN 'Unknown'
         WHEN salary >= 90000 THEN 'High'
         WHEN salary >= 70000 THEN 'Mid'
         ELSE 'Entry'
       END AS salary_band
FROM employees;
```

Order the conditions from most specific/highest threshold downward.

### 16. Above-average salary

```sql
SELECT employee_id, name, salary
FROM employees
WHERE salary > (
  SELECT AVG(salary)
  FROM employees
);
```

The average ignores NULL salaries; rows with NULL salary do not pass the comparison.

### 17. Second-highest distinct salary

```sql
SELECT MAX(salary) AS second_highest_salary
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```

If fewer than two distinct non-NULL salaries exist, the result is NULL.

Alternative using a window function:

```sql
WITH ranked AS (
  SELECT salary,
         DENSE_RANK() OVER (ORDER BY salary DESC) AS salary_rank
  FROM employees
  WHERE salary IS NOT NULL
)
SELECT MAX(salary) AS second_highest_salary
FROM ranked
WHERE salary_rank = 2;
```

### 18. COUNT variants

- `COUNT(*)`: counts all rows.
- `COUNT(salary)`: counts rows where salary is not NULL.
- `COUNT(DISTINCT salary)`: counts distinct non-NULL salary values.

## Section E — CTEs and window functions

### 19. Rank within department, ties without gaps

```sql
SELECT employee_id, name, department_id, salary,
       DENSE_RANK() OVER (
         PARTITION BY department_id
         ORDER BY salary DESC
       ) AS salary_rank
FROM employees;
```

`DENSE_RANK` gives tied salaries the same rank and has no gaps.

### 20. Highest-paid employee per department

```sql
WITH ranked AS (
  SELECT employee_id, name, department_id, salary,
         RANK() OVER (
           PARTITION BY department_id
           ORDER BY salary DESC
         ) AS salary_rank
  FROM employees
  WHERE salary IS NOT NULL
)
SELECT employee_id, name, department_id, salary
FROM ranked
WHERE salary_rank = 1;
```

`RANK() = 1` returns all tied top earners. Use `ROW_NUMBER()` with a deterministic tie-breaker if exactly one employee per department is required.

### 21. Running total by order date and ID

```sql
SELECT order_id, order_date, amount,
       SUM(amount) OVER (
         ORDER BY order_date, order_id
         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
       ) AS running_amount
FROM orders;
```

The explicit `ROWS` frame and unique tie-breaker make row-by-row accumulation clear.

### 22. Daily revenue compared with previous day

```sql
WITH daily AS (
  SELECT order_date, SUM(amount) AS revenue
  FROM orders
  GROUP BY order_date
),
compared AS (
  SELECT order_date, revenue,
         LAG(revenue) OVER (ORDER BY order_date) AS previous_revenue
  FROM daily
)
SELECT order_date, revenue, previous_revenue,
       revenue - previous_revenue AS change_from_previous
FROM compared
ORDER BY order_date;
```

This compares with the previous date present in the data, not necessarily the previous calendar day. A calendar table is needed to fill missing dates.

### 23. Latest order per customer

```sql
WITH ranked AS (
  SELECT order_id, customer_id, order_date, amount,
         ROW_NUMBER() OVER (
           PARTITION BY customer_id
           ORDER BY order_date DESC, order_id DESC
         ) AS rn
  FROM orders
)
SELECT order_id, customer_id, order_date, amount
FROM ranked
WHERE rn = 1;
```

The order ID is a deterministic tie-breaker when dates match.

## Section F — Concepts and performance

### 24. Primary key versus foreign key

A primary key uniquely identifies each row and cannot be NULL. A foreign key references a key in another table and enforces referential integrity when configured. A table can have multiple foreign keys but one primary-key constraint (possibly composite).

### 25. Indexes

An index is a data structure that helps the database locate rows without scanning the entire table. It can speed up selective filters, joins, and some sorts. Trade-offs include storage use, maintenance overhead on writes, and potential planner choices that are not beneficial for every query.

### 26. Normalization

Normalization organizes data to reduce duplication and update anomalies by separating entities into related tables. Denormalization intentionally duplicates or precomputes data to improve read performance or simplify analytics, at the cost of more storage and consistency work.

### 27. Slow query after a join — checklist

1. Confirm the join keys and expected relationship cardinality.
2. Check whether duplicate keys multiply rows.
3. Select only required columns.
4. Filter early where semantically correct.
5. Inspect indexes on join and filter columns.
6. Review the query plan (`EXPLAIN` / engine-specific equivalent).
7. Check table sizes, statistics, and whether an aggregation can happen before the join.
8. Compare row counts and timings before and after each change.

### 28. `NOT IN` and NULL

If a `NOT IN` subquery returns NULL, comparisons can evaluate to UNKNOWN and unexpectedly return no rows. `NOT EXISTS` is often safer:

```sql
SELECT c.customer_id
FROM customers AS c
WHERE NOT EXISTS (
  SELECT 1
  FROM orders AS o
  WHERE o.customer_id = c.customer_id
);
```

### 29. UNION versus UNION ALL

`UNION` combines results and removes duplicate rows; `UNION ALL` keeps duplicates and is usually cheaper. Both queries must return compatible column counts and types.

### 30. LEFT JOIN rows disappearing

A `WHERE` predicate on the right-side table filters out rows where right-side columns are NULL, effectively making the result behave like an inner join for that condition. If unmatched left rows must remain, move the right-side predicate into `ON`:

```sql
SELECT l.id, r.status
FROM left_table AS l
LEFT JOIN right_table AS r
  ON r.left_id = l.id
 AND r.status = 'active';
```

## Bonus — Cohort-style summary

One SQLite-compatible approach, assuming signup dates and event dates are stored in ISO `YYYY-MM-DD` format:

```sql
WITH user_activity AS (
  SELECT u.user_id,
         strftime('%Y-%m', u.signup_date) AS signup_month,
         CASE WHEN EXISTS (
           SELECT 1
           FROM events AS e
           WHERE e.user_id = u.user_id
             AND date(e.event_date) >= date(u.signup_date)
             AND date(e.event_date) < date(u.signup_date, '+7 days')
         ) THEN 1 ELSE 0 END AS active_within_7_days
  FROM users AS u
)
SELECT signup_month,
       COUNT(*) AS signups,
       SUM(active_within_7_days) AS users_with_event_within_7_days
FROM user_activity
GROUP BY signup_month
ORDER BY signup_month;
```

This defines the window as signup time through, but not including, the timestamp/date seven days later. If the requirement is inclusive of day seven, adjust the boundary. `EXISTS` ensures multiple events do not count the same user more than once.
