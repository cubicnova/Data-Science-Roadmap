# Practice Exercises — Sorting and Limiting in SQL

Use the sample tables from `README.md` or create your own. Include tie-breakers when deterministic results matter.

1. Sort all products by price from low to high.
2. Sort all products by price from high to low.
3. Sort students alphabetically by name.
4. Sort students by department, then age from highest to lowest.
5. Sort orders by newest order date first.
6. Sort orders by date ascending and total descending as a tie-breaker.
7. Return the five most expensive products.
8. Return the three cheapest products, using product ID to break ties.
9. Return the ten most recent orders.
10. Return rows 11–20 from a sorted product list using LIMIT and OFFSET.
11. Explain why LIMIT without ORDER BY can produce unpredictable results.
12. Sort by a computed discounted price and show the original price too.
13. Sort by the length of a product name from shortest to longest.
14. Sort student records so non-NULL scores appear first and scores are descending.
15. Write a CASE-based sort that places missing email values last.
16. Write the SQL Server style syntax to return the top five products by price.
17. Write a query using FETCH FIRST to return the first 10 sorted students.
18. Explain the difference between ascending and descending sorting.
19. Use a second sort key to alphabetically order names when scores tie.
20. Return the top 10 customers by total spending, with customer ID as a tie-breaker.
21. Return the five oldest students, with student ID breaking age ties.
22. Use keyset pagination to return the next 10 products after product ID 500.
23. Explain one weakness of OFFSET pagination on a frequently changing table.
24. Describe how an index may help an ORDER BY query.
25. Write a query that returns the highest-scoring student per a sorted list; clarify how ties are handled.
26. Explain when RANK or DENSE_RANK is more suitable than LIMIT.
27. Sort by a date column that is stored as ISO-format text and explain the assumption required.
28. Write a query to return the most recent order for each customer using a window function.
29. Return products sorted by category ascending, price descending, then product ID ascending.
30. Sort rows with NULL prices last using a CASE expression.
31. Return a random sample of five rows in SQLite.
32. Explain why physical row order should not be relied upon.
33. Create a query with a deterministic top-five result, even when prices tie.
34. Write a pagination query and explain what the limit and offset values mean.
35. Build a SQLite demo table and test sorting, multi-column ordering, limiting, and pagination.
