# Solutions: SQL Joins

These answers use the sample tables in `examples.ipynb`.

## 1. Each order with customer name

```sql
SELECT o.order_id, o.customer_id, c.customer_name
FROM orders AS o
JOIN customers AS c ON c.customer_id = o.customer_id;
```

## 2. All customers and their orders

```sql
SELECT c.customer_name, o.order_id, o.amount
FROM customers AS c
LEFT JOIN orders AS o ON c.customer_id = o.customer_id;
```

## 3. Customers who never ordered

```sql
SELECT c.customer_id, c.customer_name
FROM customers AS c
LEFT JOIN orders AS o ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

## 4. Orders with a matching customer

```sql
SELECT o.*
FROM orders AS o
JOIN customers AS c ON c.customer_id = o.customer_id;
```

## 5. Customer, city, order, and amount

```sql
SELECT c.customer_name, c.city, o.order_id, o.amount
FROM customers AS c
JOIN orders AS o ON c.customer_id = o.customer_id;
```

## 6. Indore customers and their orders

```sql
SELECT c.customer_name, c.city, o.order_id
FROM customers AS c
LEFT JOIN orders AS o ON c.customer_id = o.customer_id
WHERE c.city = 'Indore';
```

## 7. Completed orders with customer names

```sql
SELECT c.customer_name, o.order_id, o.amount
FROM customers AS c
JOIN orders AS o ON c.customer_id = o.customer_id
WHERE o.status = 'completed';
```

## 8. All customers and only completed orders

```sql
SELECT c.customer_name, o.order_id, o.status
FROM customers AS c
LEFT JOIN orders AS o
  ON c.customer_id = o.customer_id
 AND o.status = 'completed';
```

## 9. INNER JOIN versus LEFT JOIN

INNER JOIN returns only matching rows. LEFT JOIN returns every left-side row, using NULL for right-side columns when there is no match.

## 10. CROSS JOIN

A CROSS JOIN returns every possible combination. If the two tables have m and n rows, the result contains m × n rows.

## 11. Every customer-product combination

```sql
SELECT c.customer_name, p.product_name
FROM customers AS c CROSS JOIN products AS p;
```

## 12. Count orders per customer, including zero

```sql
SELECT c.customer_id, c.customer_name, COUNT(o.order_id) AS order_count
FROM customers AS c LEFT JOIN orders AS o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;
```

## 13. Total order amount per customer, including zero

```sql
SELECT c.customer_id, c.customer_name, COALESCE(SUM(o.amount), 0) AS total_amount
FROM customers AS c LEFT JOIN orders AS o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;
```

## 14. Customers with total order amount above 500

```sql
SELECT c.customer_id, c.customer_name, SUM(o.amount) AS total_amount
FROM customers AS c JOIN orders AS o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING SUM(o.amount) > 500;
```

## 15. Orders over 200 with customer name

```sql
SELECT c.customer_name, o.order_id, o.amount
FROM customers AS c JOIN orders AS o ON c.customer_id = o.customer_id
WHERE o.amount > 200;
```

## 16. Customers with a cancelled order

```sql
SELECT DISTINCT c.customer_id, c.customer_name
FROM customers AS c JOIN orders AS o ON c.customer_id = o.customer_id
WHERE o.status = 'cancelled';
```

## 17. Customers with no cancelled orders

```sql
SELECT c.customer_id, c.customer_name
FROM customers AS c
WHERE NOT EXISTS (
  SELECT 1 FROM orders AS o
  WHERE o.customer_id = c.customer_id AND o.status = 'cancelled'
);
```

## 18. Orders sorted by amount

```sql
SELECT c.customer_name, o.order_id, o.amount
FROM customers AS c JOIN orders AS o ON c.customer_id = o.customer_id
ORDER BY o.amount DESC, o.order_id ASC;
```

## 19. Three largest orders

```sql
SELECT c.customer_name, o.order_id, o.amount
FROM customers AS c JOIN orders AS o ON c.customer_id = o.customer_id
ORDER BY o.amount DESC, o.order_id ASC LIMIT 3;
```

## 20. Average order amount per customer

```sql
SELECT c.customer_id, c.customer_name, AVG(o.amount) AS average_order_amount
FROM customers AS c LEFT JOIN orders AS o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;
```

## 21. Count orders by status

```sql
SELECT status, COUNT(*) AS order_count FROM orders GROUP BY status;
```

## 22. Total amount by status

```sql
SELECT status, SUM(amount) AS total_amount FROM orders GROUP BY status;
```

## 23. Customers with more than one order

```sql
SELECT c.customer_id, c.customer_name, COUNT(o.order_id) AS order_count
FROM customers AS c JOIN orders AS o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING COUNT(o.order_id) > 1;
```

## 24. Orders with customer names and cities

```sql
SELECT o.order_id, c.customer_name, c.city, o.amount
FROM orders AS o JOIN customers AS c ON c.customer_id = o.customer_id;
```

## 25. Why non-unique join fields can multiply rows

If a key occurs more than once in either table, each matching combination is returned. Check uniqueness and the intended one-to-one or one-to-many relationship.

## 26. Why NULL = NULL does not match

In standard SQL, equality comparisons involving NULL evaluate to unknown, not true. Use IS NULL for null checks.

## 27. Customers sharing a city with another customer

```sql
SELECT DISTINCT c1.customer_name, c1.city
FROM customers AS c1 JOIN customers AS c2
  ON c1.city = c2.city AND c1.customer_id <> c2.customer_id;
```

## 28. Unique pairs from the same city

```sql
SELECT c1.customer_name AS customer_1, c2.customer_name AS customer_2, c1.city
FROM customers AS c1 JOIN customers AS c2
  ON c1.city = c2.city AND c1.customer_id < c2.customer_id;
```

## 29. Orders whose customer city is Pune

```sql
SELECT o.order_id, o.amount, c.customer_name
FROM orders AS o JOIN customers AS c ON c.customer_id = o.customer_id
WHERE c.city = 'Pune';
```

## 30. Customer order flag

```sql
SELECT c.customer_id, c.customer_name,
       CASE WHEN COUNT(o.order_id) > 0 THEN 'Yes' ELSE 'No' END AS has_order
FROM customers AS c LEFT JOIN orders AS o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;
```

## 31. Unique customers who placed orders

```sql
SELECT COUNT(DISTINCT customer_id) AS customers_with_orders FROM orders;
```

## 32. Distinct statuses per customer

```sql
SELECT c.customer_id, c.customer_name, COUNT(DISTINCT o.status) AS distinct_status_count
FROM customers AS c LEFT JOIN orders AS o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;
```

## 33. How to check for unexpected row multiplication

Compare row counts before and after joining, inspect join-key uniqueness, and compare distinct primary-key counts. Investigate any increase not explained by the relationship cardinality.

## 34. Filter in ON versus WHERE for LEFT JOIN

```sql
-- ON condition preserves every customer, matching only completed orders
SELECT c.customer_name, o.order_id
FROM customers AS c LEFT JOIN orders AS o
  ON c.customer_id = o.customer_id AND o.status = 'completed';
```

## 35. Total order amount by city

```sql
SELECT c.city, COALESCE(SUM(o.amount), 0) AS total_order_amount
FROM customers AS c LEFT JOIN orders AS o ON c.customer_id = o.customer_id
GROUP BY c.city ORDER BY total_order_amount DESC;
```

