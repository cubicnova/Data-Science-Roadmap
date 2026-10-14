# Practice Exercises — GroupBy and Aggregation in Pandas

**Instructions:** Use Pandas and small datasets. For each question, inspect the result and explain what one output row represents.

1. Create a sales DataFrame with region, product, units, and revenue columns.
2. Calculate total revenue for each region.
3. Calculate average units sold for each product.
4. Find the minimum and maximum revenue for each region.
5. Count the number of rows in each region using size().
6. Count non-missing revenue values per region using count().
7. Explain the difference between size() and count().
8. Group by both region and product and sum revenue.
9. Return group keys as regular columns using as_index=False.
10. Calculate sum, mean, minimum, and maximum revenue in one aggregation.
11. Use named aggregation to produce total_revenue and average_revenue columns.
12. Calculate total units and distinct product count for each region.
13. Count unique salespeople in each region.
14. Calculate median revenue by product.
15. Calculate standard deviation of revenue by region.
16. Sort a groupby summary by total revenue descending.
17. Reset the index of a groupby result.
18. Use transform() to add each region's total revenue to every row.
19. Calculate each transaction's share of its region's revenue.
20. Add a column containing the average revenue for that row's region.
21. Filter to keep only regions whose total revenue is at least 1500.
22. Group by department and role to calculate average salary.
23. Group by a date column's month and sum daily sales after parsing dates.
24. Include missing region values as a group using dropna=False.
25. Demonstrate how sort=False affects the order of group keys.
26. Create a custom aggregation that returns the range (max minus min).
27. Compare a groupby aggregation with a transform result in terms of row count.
28. Calculate each product's percentage contribution to total revenue.
29. Find the top revenue-generating product within each region.
30. Create a report with transaction count, total revenue, and average revenue per region.
31. Use agg() to apply different functions to different columns.
32. Verify that the sum of regional revenue equals overall revenue.
33. Group by customer and calculate total spend and number of transactions.
34. Create a groupby summary, rename output columns, and sort it for a report.
35. Build an end-to-end analysis of revenue and units by region and product with readable column names.
