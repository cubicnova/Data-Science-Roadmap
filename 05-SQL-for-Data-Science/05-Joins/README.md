# SQL Joins

SQL joins combine rows from related tables. This module covers `INNER JOIN`, `LEFT JOIN`, `CROSS JOIN`, self joins, join conditions, NULL behavior, and common mistakes.

## Learning objectives
- Choose the right join type for a question.
- Join tables using primary and foreign keys.
- Identify unmatched records.
- Understand how join filters in `ON` differ from filters in `WHERE`.
- Aggregate results after joining tables.
- Detect accidental row multiplication.

## Sample schema

The notebook creates `customers(customer_id, customer_name, city)`, `orders(order_id, customer_id, order_date, amount, status)`, and `products(product_id, product_name, category)`.

## 1. INNER JOIN

Returns only rows that match the join condition in both tables.

```sql
SELECT c.customer_name, o.order_id, o.amount
FROM customers AS c
INNER JOIN orders AS o
  ON c.customer_id = o.customer_id;
```

## 2. LEFT JOIN

Returns every row from the left table plus matching rows from the right. Unmatched right-side fields are `NULL`.

```sql
SELECT c.customer_name, o.order_id
FROM customers AS c
LEFT JOIN orders AS o
  ON c.customer_id = o.customer_id;
```

Find customers without orders:

```sql
SELECT c.customer_id, c.customer_name
FROM customers AS c
LEFT JOIN orders AS o
  ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

## 3. RIGHT JOIN and FULL OUTER JOIN

`RIGHT JOIN` preserves all rows from the right table. `FULL OUTER JOIN` preserves unmatched rows from both tables as well as matches. Support depends on the database and version; for portable SQL, a right join can often be expressed by reversing table order in a left join.

## 4. CROSS JOIN

Returns every possible combination of rows from both tables.

```sql
SELECT c.customer_name, p.product_name
FROM customers AS c
CROSS JOIN products AS p;
```

If one table has 4 rows and another has 3, the result has 12 rows. Use this intentionally.

## 5. Self join

A self join joins a table to itself, such as when connecting employees to managers. Give each table instance a distinct alias.

```sql
SELECT e.employee_name AS employee,
       m.employee_name AS manager
FROM employees AS e
LEFT JOIN employees AS m
  ON e.manager_id = m.employee_id;
```

## 6. Filter placement with LEFT JOIN

The `ON` clause defines matching; `WHERE` filters the joined result. To preserve customers even when they have no completed orders, put the status condition in `ON`:

```sql
SELECT c.customer_name, o.order_id
FROM customers AS c
LEFT JOIN orders AS o
  ON c.customer_id = o.customer_id
 AND o.status = 'completed';
```

A right-side condition in `WHERE` can exclude unmatched rows and effectively turn that part of the query into an inner join.

## 7. Joins and aggregation

```sql
SELECT c.customer_name,
       COUNT(o.order_id) AS order_count,
       COALESCE(SUM(o.amount), 0) AS total_amount
FROM customers AS c
LEFT JOIN orders AS o
  ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;
```

For customers with no orders, `COUNT(o.order_id)` returns zero. `COUNT(*)` would count the preserved left-side row, so it is not interchangeable here.

## Common pitfalls
- Missing an `ON` condition can produce a Cartesian product.
- One-to-many joins naturally repeat a parent row.
- Joining on a non-unique key can multiply matching combinations.
- Standard equality comparisons do not match `NULL` to `NULL`; use `IS NULL` for null checks.
- `DISTINCT` should not be used to hide unexplained duplicates.
- Qualify ambiguous column names and select only the columns you need.

## Best practices
1. Identify primary and foreign keys first.
2. Use consistent table aliases.
3. Know whether each relationship is one-to-one, one-to-many, or many-to-many.
4. Compare row counts and distinct primary keys before and after joining.
5. Test unmatched cases with a left join.
6. Use query-plan tools when optimizing performance.

Run `examples.ipynb`, answer the questions in `practice.md`, and then check `solutions.md`.
