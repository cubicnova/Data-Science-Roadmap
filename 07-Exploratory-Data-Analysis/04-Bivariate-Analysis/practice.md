# Practice Exercises — Bivariate Analysis

Use `examples.ipynb` as your starting point. Try the tasks before checking `solution.md`. The notebook generates a synthetic student dataset with numerical and categorical columns.

## Dataset overview

The DataFrame `df` includes:

- `study_hours`: hours studied per week
- `attendance_pct`: attendance percentage
- `exam_score`: exam score out of 100
- `department`: student's department
- `study_mode`: study mode
- `satisfaction`: ordinal satisfaction rating
- `scholarship`: scholarship status

## Level 1 — Choose the right method

1. Explain the difference between univariate and bivariate analysis.
2. Classify these pairs by variable type: `study_hours` vs `exam_score`, `department` vs `scholarship`, and `department` vs `exam_score`.
3. For each pair, choose one chart and one numerical summary that could help answer a question.
4. Count missing values for the variables used in each analysis.

## Level 2 — Numerical vs numerical

5. Create a scatter plot of `study_hours` versus `exam_score`.
6. Create a scatter plot of `attendance_pct` versus `exam_score`.
7. Calculate Pearson correlation between study hours and exam score.
8. Calculate Spearman correlation between study hours and exam score.
9. Compare the two coefficients. Explain why they may differ.
10. Calculate covariance between attendance and exam score.
11. Add a regression trend line to the attendance-score scatter plot.
12. Identify any unusual observations that may affect the relationship.

## Level 3 — Numerical vs categorical

13. Calculate count, mean, median, and standard deviation of `exam_score` for each department.
14. Sort departments by median exam score.
15. Create a box plot of exam scores by department.
16. Compare exam scores across study modes using grouped summaries and a box plot.
17. Compare mean and median scores for each group. Why is it useful to report both?
18. Explain why differences between group averages do not, by themselves, prove that department or study mode causes differences in exam scores.

## Level 4 — Categorical vs categorical

19. Create a cross-tabulation of `department` and `scholarship`.
20. Calculate row percentages for the cross-tabulation.
21. Create a stacked bar chart showing scholarship status by department.
22. Which department has the highest scholarship share? Support the answer with the computed percentages and counts.
23. Explain why percentages can be misleading when group sizes are small.

## Level 5 — Interpret and communicate

24. Create a correlation matrix for `study_hours`, `attendance_pct`, and `exam_score`.
25. Write three evidence-based observations about relationships between pairs of variables.
26. Identify at least two possible confounding variables or alternative explanations for a study-hours/exam-score association.
27. Write a 150–250 word report including methods, results, caveats, and next steps.
28. Explain the difference between correlation and causation using one relationship in the dataset.

## Optional challenge

29. Create a reusable function that takes two column names and returns a suitable basic analysis depending on their data types.
30. Compare Pearson and Spearman correlation after checking for extreme values. Describe how the result changes, if at all, and why the choice to remove observations must be justified.
31. Use `pd.crosstab(..., normalize="index")` to compare category proportions and verify that each non-empty row sums to 1.
32. Investigate whether the study-hours/exam-score relationship appears different across departments. Treat this as a subgroup exploration, not causal evidence.

## Submission checklist

- [ ] Each analysis includes exactly two primary variables.
- [ ] Charts have clear titles and axis labels.
- [ ] Missing-value handling is documented.
- [ ] Group counts accompany comparisons.
- [ ] Correlation is interpreted as association, not causation.
- [ ] Findings are supported by outputs and limitations are disclosed.
