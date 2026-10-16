# Practice Exercises — EDA Case Studies

Use `examples.ipynb` as a guide, but try to complete each task independently. The notebook creates synthetic datasets for retail sales, student performance, and customer churn.

## Case Study 1 — Retail Sales Performance

### Business brief
A retail manager wants to understand sales distribution across regions and categories and identify data-quality issues before planning inventory.

1. Write a one-paragraph problem statement and list three questions for the analysis.
2. Inspect the dataset shape, columns, data types, and first five records.
3. Calculate missing-value counts and percentages.
4. Check for duplicate records and explain what defines a duplicate in this dataset.
5. Calculate total sales, average transaction value, and order count by region.
6. Calculate total sales and average transaction value by product category.
7. Plot total sales by region and category.
8. Plot the distribution of transaction amounts and inspect potential outliers.
9. Compare average and median transaction values. What might a difference suggest?
10. Write three findings and two recommendations. Include supporting values and caveats.

## Case Study 2 — Student Performance

### Business brief
An academic coordinator wants to explore score distributions and see whether attendance and study hours are associated with exam scores.

11. Inspect missing values in study hours, attendance, and exam score.
12. Calculate mean, median, standard deviation, and quartiles for exam scores.
13. Visualize exam score and study-hour distributions.
14. Compare score distributions by department using group summaries and a box plot.
15. Calculate Pearson and Spearman correlations between study hours and exam score.
16. Plot attendance against exam score and describe the observed pattern.
17. Identify at least two confounders or alternative explanations.
18. Explain why these data cannot establish that study hours cause higher scores.
19. Write a short report with three findings and two recommended follow-up analyses.

## Case Study 3 — Customer Churn Exploration

### Business brief
A subscription service wants to explore how tenure and monthly charges differ between customers who churned and those who stayed.

20. Calculate the churn count and percentage.
21. Inspect tenure and monthly-charge distributions.
22. Compare median tenure and median monthly charges by churn status.
23. Create box plots of tenure and monthly charges by churn status.
24. Compare churn percentages across contract types.
25. Calculate group sizes before interpreting percentage differences.
26. List three patterns that deserve further investigation.
27. Explain why group differences do not prove that a variable causes churn.
28. Recommend two additional fields that would help investigate churn, and explain why.

## Cross-case-study tasks

29. For each case study, document the objective, data quality, methods, three findings, limitations, and next steps.
30. Identify one chart that is useful and one that would be misleading for each business question.
31. Choose one finding from each case and rewrite it as a precise, evidence-based statement.
32. Write a 250–400 word executive summary comparing insights and limitations across the three cases.

## Optional challenge

33. Create a reusable `eda_profile(data)` function returning shape, types, missing counts/percentages, duplicate count, numeric summaries, and categorical frequency tables.
34. Save one chart per case study to a `figures/` directory using clear filenames.
35. Create a single report notebook with a table of contents and a consistent style for all case studies.
36. Explain how you would validate the findings using real production data before making a business decision.

## Submission checklist

- [ ] Each case starts with a clearly stated question.
- [ ] Data quality is checked before conclusions are drawn.
- [ ] Charts have titles, axis labels, and readable categories.
- [ ] Comparisons include group counts or denominators.
- [ ] Findings are supported by calculated values.
- [ ] Recommendations are connected to findings.
- [ ] Synthetic-data limitations and causal caveats are explicit.
