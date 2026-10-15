# SQL Practice Problems

A hands-on SQL problem set for Data Science and Data Analytics learners. The exercises progress from basic retrieval to joins, aggregation, subqueries, CTEs, window functions, data cleaning, and business analysis.

## How to use this module
1. Read `practice.md` and attempt each problem before opening `solutions.md`.
2. Run `examples.ipynb` to create a small SQLite database and practice queries.
3. Test your answers against edge cases: duplicate values, missing values, and empty result sets.
4. Record alternative solutions and explain why they work.

## Dataset
The notebook creates a small retail dataset with:
- `customers(customer_id, customer_name, city)`
- `orders(order_id, customer_id, order_date, status)`
- `products(product_id, product_name, category, price)`
- `order_items(order_id, product_id, quantity, unit_price)`

The SQL in the notebook uses SQLite. Most concepts transfer to other relational databases, but date functions, limiting syntax, and some type/constraint details may vary.

## Topic map
- SELECT, WHERE, DISTINCT, ORDER BY, LIMIT
- String and NULL handling
- JOINs and relationship reasoning
- GROUP BY, HAVING, aggregates
- CASE expressions
- Subqueries and CTEs
- Window functions and ranking
- Date filtering and business questions

## Files
- `README.md` — module overview and dataset
- `practice.md` — 40 exercises
- `solutions.md` — reference solutions with SQL
- `examples.ipynb` — runnable, executed SQLite examples

## Suggested workflow
Solve questions in order for a gradual path, or pick a topic based on the concept you are studying. Use `EXPLAIN QUERY PLAN` in SQLite (or the equivalent in your database) when you begin exploring query performance.
