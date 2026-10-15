# SQL Common Table Expressions (CTEs)

A Common Table Expression (CTE) is a named temporary result set defined with `WITH` and used by a single SQL statement. CTEs can make complex queries easier to read by breaking them into logical steps.

## Learning objectives
- Define and use a CTE with `WITH`.
- Use multiple CTEs in one query.
- Use CTEs with aggregates, joins, and subqueries.
- Understand recursive CTEs.
- Compare CTEs with derived tables and temporary tables.
- Avoid common syntax and logic errors.

## 1. Basic CTE

```sql
WITH high_earners AS (
    SELECT employee_name, department_id, salary
    FROM employees
    WHERE salary >= 60000
)
SELECT *
FROM high_earners
ORDER BY salary DESC;
```

The CTE is available to the statement that immediately follows it. It is not a permanent table.

## 2. CTE with aggregation

```sql
WITH department_stats AS (
    SELECT department_id,
           COUNT(*) AS employee_count,
           AVG(salary) AS average_salary
    FROM employees
    GROUP BY department_id
)
SELECT *
FROM department_stats
WHERE average_salary > 60000;
```

The inner query creates a summarized result; the outer query filters that result.

## 3. Multiple CTEs

Separate CTEs with commas, then reference them in the final query or in later CTEs.

```sql
WITH department_stats AS (
    SELECT department_id, AVG(salary) AS average_salary
    FROM employees
    GROUP BY department_id
),
high_departments AS (
    SELECT department_id, average_salary
    FROM department_stats
    WHERE average_salary > 60000
)
SELECT d.department_name, h.average_salary
FROM high_departments AS h
JOIN departments AS d
  ON d.department_id = h.department_id;
```

A later CTE can refer to an earlier CTE in the same `WITH` clause.

## 4. CTEs with joins

```sql
WITH customer_totals AS (
    SELECT customer_id, SUM(amount) AS total_spend
    FROM orders
    GROUP BY customer_id
)
SELECT c.customer_name, COALESCE(t.total_spend, 0) AS total_spend
FROM customers AS c
LEFT JOIN customer_totals AS t
  ON t.customer_id = c.customer_id;
```

This pattern can make a report clearer than embedding a long aggregate subquery inside a join.

## 5. Recursive CTEs

A recursive CTE refers to itself and is useful for hierarchies, trees, and sequences. It has an anchor query and a recursive query, typically joined with `UNION ALL`.

```sql
WITH RECURSIVE numbers(n) AS (
    SELECT 1
    UNION ALL
    SELECT n + 1
    FROM numbers
    WHERE n < 5
)
SELECT n FROM numbers;
```

The anchor starts at 1; the recursive term adds one until the condition stops recursion. Always ensure recursion has a safe termination condition.

## 6. CTEs for running totals

Window functions can be combined with CTEs to separate calculation steps.

```sql
WITH monthly_sales AS (
    SELECT month, SUM(amount) AS revenue
    FROM sales
    GROUP BY month
)
SELECT month, revenue,
       SUM(revenue) OVER (ORDER BY month) AS running_revenue
FROM monthly_sales
ORDER BY month;
```

## 7. CTE versus derived table versus temporary table

- **CTE:** named result scoped to one statement; useful for readability and multi-step logic.
- **Derived table:** subquery in `FROM`; also scoped to one statement, but nested inline.
- **Temporary table:** a database object that can be reused across multiple statements in its session, depending on the database.

A CTE is not automatically faster than a subquery. Optimizers may inline or materialize it depending on the database and query.

## 8. Common mistakes
- Forgetting the final query after the CTE definition.
- Missing a comma between multiple CTE definitions.
- Referencing a CTE before it is defined in a non-recursive `WITH` clause.
- Expecting a CTE to persist across separate SQL statements.
- Writing a recursive CTE without a stopping condition.
- Using `UNION` when `UNION ALL` is intended, or vice versa.
- Assuming CTE materialization behavior is identical across database engines.

## Best practices
- Give each CTE a meaningful name.
- Keep each CTE focused on one transformation.
- Select only the columns needed downstream.
- Use explicit column names and aliases.
- Test each logical step separately when debugging.
- Add a termination condition to recursive CTEs.
- Inspect the query plan for performance-sensitive queries.

Run `examples.ipynb`, complete the 35 exercises in `practice.md`, and review `solutions.md`.
