# SQL Subqueries

A subquery is a query nested inside another SQL statement. It can return a scalar value, a list of values, a derived table, or a Boolean result.

## Main types
- **Scalar subquery:** returns one value, e.g. `salary > (SELECT AVG(salary) FROM employees)`.
- **Multi-row subquery:** returns a set of values, often used with `IN`.
- **EXISTS / NOT EXISTS:** tests whether related rows exist.
- **Correlated subquery:** references a column from the outer query.
- **Derived table:** a subquery in `FROM`, which should have an alias.

## Scalar example
```sql
SELECT employee_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

## IN example
```sql
SELECT employee_name
FROM employees
WHERE department_id IN (
  SELECT department_id FROM departments WHERE location = 'Indore'
);
```

## EXISTS example
```sql
SELECT d.department_name
FROM departments AS d
WHERE EXISTS (
  SELECT 1 FROM employees AS e WHERE e.department_id = d.department_id
);
```

## Correlated subquery
```sql
SELECT e.employee_name, e.salary
FROM employees AS e
WHERE e.salary > (
  SELECT AVG(e2.salary) FROM employees AS e2
  WHERE e2.department_id = e.department_id
);
```

## Derived table
```sql
SELECT department_id, average_salary
FROM (
  SELECT department_id, AVG(salary) AS average_salary
  FROM employees GROUP BY department_id
) AS department_stats
WHERE average_salary > 50000;
```

## NULL and performance
A `NOT IN` subquery that returns `NULL` can produce unexpected results. `NOT EXISTS` is often safer for anti-matching. A correlated subquery is not automatically slow: the optimizer may transform it, so inspect query plans and test on representative data. Index columns used in filters and correlation where appropriate.

## Best practices
- Ensure a scalar subquery returns at most one row.
- Use `IN` for membership and `EXISTS` for presence checks.
- Use explicit aliases for outer and inner tables.
- Be clear about which rows contribute to aggregates.
- Compare query plans rather than assuming joins or subqueries are always faster.

Run `examples.ipynb`, attempt all 35 questions in `practice.md`, then check `solutions.md`.
