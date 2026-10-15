# SQL Window Functions — 35 Practice Exercises

Use the sample `sales` table described in `README.md` unless an exercise specifies another table. Assume common SQL syntax; minor adaptations may be needed for your database engine.

1. Explain what a SQL window function does and how it differs from a `GROUP BY` aggregate.
2. Explain the purpose of `OVER()`.
3. What is the difference between `PARTITION BY` and `ORDER BY` inside `OVER()`?
4. Given amounts 80, 80, and 60 in descending order, show the outputs of `ROW_NUMBER`, `RANK`, and `DENSE_RANK` (assume a deterministic tie-breaker for `ROW_NUMBER`).
5. Write a query that numbers all sales by sale date and sale ID.
6. Write a query that ranks employees by salary within each department, highest first.
7. Write a query that uses `DENSE_RANK` to identify the three highest distinct salary levels in each department.
8. Write a query that calculates total sales per region while retaining every sale row.
9. Write a query that calculates a running total of `amount`, ordered by `sale_date` and `sale_id`.
10. Why is an explicit `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` useful for a running total?
11. Write a query to calculate the average sale amount for each salesperson on every row.
12. Write a query to calculate a three-row moving average of `amount`.
13. Explain the difference between `ROWS BETWEEN 2 PRECEDING AND CURRENT ROW` and a whole-partition average.
14. Write a query using `LAG` to show the previous sale amount.
15. Write a query using `LEAD` to show the next sale amount.
16. Write a query to calculate the absolute and percentage change from the previous row's amount. Handle a missing previous row and avoid division by zero.
17. Write a query that returns each salesperson's first sale amount using `FIRST_VALUE`.
18. Why can `LAST_VALUE` return the current row's value instead of the last value in the whole partition?
19. Write a query that gets the final amount for each salesperson using an explicit full-partition frame.
20. Write a query that calculates each row's percentage of total sales.
21. Write a query using `NTILE(4)` to divide products into four price groups.
22. Explain `RANK`, `PERCENT_RANK`, and `CUME_DIST` at a high level.
23. Write a CTE that returns the top two sales in each region.
24. Why can't a window function usually be used directly in a `WHERE` clause?
25. Rewrite a top-three-per-category query using a subquery instead of a CTE.
26. Write a query to flag duplicate email addresses by assigning `ROW_NUMBER` within each email group.
27. Write a query that compares each sale amount with the average for its region and returns the difference.
28. Write a query that counts sales per salesperson while retaining every sale row.
29. Write a query that returns the minimum and maximum sale amount per region on every row.
30. Explain how ties affect `ROW_NUMBER`, `RANK`, and `DENSE_RANK`.
31. Does `PARTITION BY region` guarantee that the final output is sorted by region? Explain.
32. Write a query that calculates a cumulative count of sales ordered by date and sale ID.
33. Write a query to return each employee's salary and the department's second-highest distinct salary level, using `DENSE_RANK` or an equivalent window expression.
34. Name three practical business uses of SQL window functions.
35. List four best practices for reliable window-function queries.
