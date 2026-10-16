# Practice Exercises — Outlier Detection

Use `examples.ipynb` as a starting point. Try each exercise before reading `solution.md`. The notebook creates a synthetic transaction dataset with deliberately unusual values.

## Dataset overview
The DataFrame `df` contains:
- `transaction_id`: transaction identifier
- `transaction_amount`: transaction amount
- `items_count`: number of items
- `customer_age`: customer age
- `region`: region label
- `payment_method`: payment method

Some numeric values are intentionally unusual. Do not assume every flagged observation is an error.

## Level 1 — Understand and inspect
1. Display the first five rows and inspect data types.
2. Find the number of rows and columns.
3. Calculate missing values for each column.
4. Use `describe()` to summarize numerical columns.
5. Plot a histogram and box plot of `transaction_amount`.
6. Explain the difference between an outlier and an invalid value.

## Level 2 — IQR method
7. Calculate Q1, Q3, and IQR for `transaction_amount`.
8. Calculate lower and upper IQR fences using 1.5 × IQR.
9. List rows flagged as outliers by the IQR rule.
10. Count flagged observations and calculate their percentage of non-missing values.
11. Repeat the analysis for `items_count`.
12. Explain how a highly skewed distribution can affect IQR-based flags.

## Level 3 — Z-score method
13. Calculate z-scores for `transaction_amount` using the mean and standard deviation.
14. Flag observations with an absolute z-score greater than 3.
15. Compare IQR and z-score flags. Which observations are found by both?
16. Explain why z-scores can be affected by extreme observations.
17. Compare the mean and standard deviation before and after excluding flagged values in a temporary copy. Do not overwrite the original.

## Level 4 — Investigate and treat
18. Inspect flagged records alongside `region` and `payment_method`.
19. Create a scatter plot of `items_count` versus `transaction_amount`.
20. Create `capped_df` and cap transaction amounts at IQR fences. Compare distributions.
21. Create `filtered_df` using the IQR rule. Compare row counts and summary statistics.
22. Describe a case where keeping a high transaction amount is more appropriate than removing it.
23. Describe a case where removing or correcting an observation is justified.

## Level 5 — Report and reflect
24. Create a summary table with Q1, Q3, IQR, fences, outlier count, and percentage for each numeric variable.
25. Write three findings supported by numbers or plots.
26. Explain why outlier treatment decisions should be documented.
27. Explain how to avoid data leakage when outlier thresholds are used in machine learning.
28. Write a 150–250 word report covering method, findings, treatment decision, and limitations.

## Optional challenge
29. Implement `iqr_outlier_mask(series, factor=1.5)` returning a Boolean mask.
30. Implement `zscore_outlier_mask(series, threshold=3)` safely handling missing values and zero standard deviation.
31. Compare IQR multipliers 1.5 and 3.0. Explain the sensitivity trade-off.
32. Calculate transaction amount per item and investigate whether unusual amounts remain unusual after accounting for item count.

## Submission checklist
- [ ] I can reproduce the outlier flags.
- [ ] I checked flagged rows and related fields.
- [ ] I did not overwrite the raw dataset.
- [ ] I compared data before and after treatment.
- [ ] I documented why a treatment is appropriate.
- [ ] I considered distribution shape and data leakage.
