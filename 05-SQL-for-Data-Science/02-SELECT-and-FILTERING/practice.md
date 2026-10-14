# Practice Exercises — SELECT and Filtering in SQL

Use the sample schema from `README.md` or create small tables of your own. Verify edge cases such as `NULL` and range boundaries.

1. Select all columns from a table named `students`.
2. Select only `student_id` and `student_name` from `students`.
3. Return each student's name under the alias `name`.
4. Select product name and price, and calculate a 10% discount price.
5. Find products priced above 500.
6. Find students whose age is exactly 20.
7. Find orders whose total is less than or equal to 1000.
8. Find students who are not in the `Data Science` department.
9. Find products with price between 100 and 500, inclusive.
10. Find students belonging to either `Data Science` or `Cybersecurity` using `IN`.
11. Find customer names beginning with `A` using `LIKE`.
12. Find product names containing the word `phone` using `LIKE`.
13. Find product codes with exactly one character between `AB` and `7` using `_`.
14. Find students whose email is missing.
15. Find students whose email is present.
16. Return distinct department names.
17. Sort products from highest to lowest price.
18. Sort students by department and then by student name.
19. Return the five most expensive products using an appropriate row limit.
20. Find orders placed during January 2026 using a half-open date interval.
21. Find products that are not in the `Accessories` category and handle possible NULL values deliberately.
22. Find students aged 18 through 25 whose department is `Computer Science`.
23. Find products that are laptops or tablets and cost less than 1000.
24. Use parentheses to filter orders that are either pending or processing and have totals over 200.
25. Return a display value of `Not provided` when a student's email is NULL using `COALESCE`.
26. Find students whose names contain exactly four characters using a suitable pattern.
27. Find names that end with `a`.
28. Explain the difference between `WHERE column = NULL` and `WHERE column IS NULL`.
29. Write a query that returns the distinct order statuses in alphabetical order.
30. Write a query that filters a price column to values greater than 0 and not NULL.
31. Write a query that sorts by newest order date first and highest total second.
32. Write a Python SQLite parameterized query to select students by department.
33. Explain why concatenating a user's search text into SQL is unsafe.
34. Write a query that returns products in a price range, sorted cheapest first, limited to 10 rows.
35. Create a small SQLite table and demonstrate SELECT, WHERE, LIKE, IS NULL, ORDER BY, and LIMIT.
