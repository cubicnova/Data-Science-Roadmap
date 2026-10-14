# Practice Exercises — Merging and Joining in Pandas

**Instructions:** Create small DataFrames for each task. Check key uniqueness and row counts before interpreting merge output.

1. Create customers and orders DataFrames with a shared customer_id key.
2. Perform an inner merge between orders and customers.
3. Perform a left merge that preserves every order.
4. Perform a right merge that preserves every customer.
5. Perform an outer merge and explain why missing values appear.
6. Merge two DataFrames when the key columns have different names.
7. Merge two tables using a composite key of customer_id and month.
8. Use indicator=True to identify rows that matched and did not match.
9. Count the left_only, right_only, and both merge indicator values.
10. Use validate='many_to_one' for an orders-to-customers merge.
11. Explain how duplicate keys on both sides can multiply output rows.
12. Check whether customer_id is unique in a customer table.
13. Find order rows whose customer_id does not exist in customers.
14. Find customers who have no matching orders.
15. Merge tables that contain overlapping non-key column names and set suffixes.
16. Join orders to customers after setting customer_id as the customer index.
17. Use DataFrame.join() to combine two DataFrames by index.
18. Use concat() to stack January and February transaction rows.
19. Use concat(axis=1) to place two index-aligned DataFrames side by side.
20. Explain the difference between merge(), join(), and concat().
21. Merge only selected columns from a larger customer table.
22. Convert merge keys to compatible data types before merging.
23. Normalize whitespace in string keys before merging.
24. Compare row counts before and after a merge.
25. Use an outer merge with an indicator to reconcile two ID lists.
26. Perform a one-to-one merge and validate the relationship.
27. Demonstrate a many-to-many merge and count the resulting rows.
28. Use suffixes to distinguish old and new versions of a column.
29. Merge product data with inventory data on product_id.
30. Combine two monthly datasets and reset the index.
31. Merge employee and department tables on department_id.
32. Use merge indicator values to create a report of unmatched records.
33. Handle unmatched values after a left merge with fillna().
34. Create a merge pipeline that validates key uniqueness and row counts.
35. Build an end-to-end report combining customers and orders with total spend per customer.
