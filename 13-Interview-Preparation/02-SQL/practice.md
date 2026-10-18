# SQL Interview Practice

Use the tables created in `examples.ipynb` unless a question supplies its own schema. Try each task before opening `solution.md`. State assumptions clearly, especially around ties, NULL values, and duplicate records.

## Section A — Fundamentals

1. Return all employees with their name, department ID, and salary, ordered from highest to lowest salary.
2. List distinct department IDs represented in the employee table.
3. Find employees whose salary is at least 80,000.
4. Find employees whose name begins with `A`.
5. Return employees whose department is unknown (`NULL`).

## Section B — Aggregation

6. Count employees in each department.
7. Return each department's average salary, highest salary first.
8. Show only departments with at least two employees.
9. Count the number of non-NULL salary values and compare it with the total row count.
10. Calculate total revenue by product category from a hypothetical `orders` table with `category` and `amount`.

## Section C — Joins

11. Return each employee's name and department name, including employees without a matching department.
12. Find departments that have no employees.
13. Return employee names alongside their manager names using a self join.
14. Explain why joining an order table to an order-items table may increase the number of rows.

## Section D — NULLs, CASE, and subqueries

15. Label each employee as `High` when salary is at least 90,000, `Mid` when at least 70,000, otherwise `Entry`; handle a missing salary explicitly.
16. Find employees paid more than the overall average salary.
17. Find the second-highest **distinct** salary.
18. Explain the difference between `COUNT(*)`, `COUNT(salary)`, and `COUNT(DISTINCT salary)`.

## Section E — CTEs and window functions

19. Rank employees by salary within each department. Use a function that gives tied salaries the same rank without gaps.
20. Return the highest-paid employee in each department. Explain how you would handle ties.
21. Show a running total of order amounts ordered by order date and order ID.
22. Use `LAG` to compare each day's revenue with the previous day's revenue.
23. Return the latest order per customer.

## Section F — Database concepts and performance

24. Explain primary key versus foreign key.
25. What is an index? Give one benefit and two trade-offs.
26. Explain normalization and why denormalization might still be useful.
27. A query becomes slow after adding a join. Give a practical debugging checklist.
28. Why can `NOT IN` behave unexpectedly when the subquery returns `NULL`? What is a common alternative?
29. Explain the difference between `UNION` and `UNION ALL`.
30. A `LEFT JOIN` returns fewer rows after you add `WHERE right_table.status = 'active'`. Why, and how might you preserve unmatched left rows?

## Bonus challenge — Cohort-style summary

Given `users(user_id, signup_date)` and `events(user_id, event_date)`, write a query that reports the number of users who signed up in each month and how many of those users had at least one event within 7 days of signup. State how you define the seven-day boundary and avoid counting one user multiple times.

## Mock interview practice

For any query you write, be ready to explain:
- The expected output grain (one row per what?).
- How duplicates and ties are handled.
- What happens when a value is `NULL`.
- Whether the query is portable across SQL dialects.
- How you would validate correctness and performance.
