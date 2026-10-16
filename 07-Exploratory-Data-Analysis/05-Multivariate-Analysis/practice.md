# Practice — Multivariate Analysis

**Instructions:** Complete these exercises using Python, pandas, NumPy, Matplotlib, and Seaborn. Use the synthetic dataset in `examples.ipynb`. Treat patterns as exploratory evidence, not proof of causation.

## Foundations

### 1. Define univariate, bivariate, and multivariate analysis and give an example of each.

**Your answer / code / interpretation:**

### 2. A dataset contains exam score, study hours, attendance, program, and prior score. Propose two multivariate questions.

**Your answer / code / interpretation:**

### 3. Explain why adding more variables to a chart can make it less useful.

**Your answer / code / interpretation:**

### 4. Explain the difference between association, interaction, and confounding.

**Your answer / code / interpretation:**

### 5. Why confirm the unit of observation before comparing several variables?

**Your answer / code / interpretation:**

## Preparation and summaries

### 6. Load the notebook dataset and print shape, dtypes, and first five rows.

**Your answer / code / interpretation:**

### 7. Select numeric columns and calculate a Pearson correlation matrix.

**Your answer / code / interpretation:**

### 8. Calculate a Spearman matrix. When may it be more informative than Pearson?

**Your answer / code / interpretation:**

### 9. Summarize exam score count, mean, median, and standard deviation by program.

**Your answer / code / interpretation:**

### 10. Create a pivot table of mean exam score by program and study group.

**Your answer / code / interpretation:**

### 11. Use crosstab to count combinations of program and result band.

**Your answer / code / interpretation:**

### 12. Calculate row percentages for program/result-band and explain the denominator.

**Your answer / code / interpretation:**

### 13. Calculate missing counts and percentages, then compare missingness by program.

**Your answer / code / interpretation:**

## Visualization

### 14. Create a correlation heatmap scaled from -1 to +1 with a descriptive title.

**Your answer / code / interpretation:**

### 15. Create a pair plot for study hours, attendance, prior score, and exam score, using program as hue.

**Your answer / code / interpretation:**

### 16. Plot study hours against exam score and color points by program.

**Your answer / code / interpretation:**

### 17. Add a third variable using size or facets; explain whether readability improves.

**Your answer / code / interpretation:**

### 18. Create a box plot of exam score by program and study group.

**Your answer / code / interpretation:**

### 19. Create a faceted plot comparing study-hours/exam-score patterns across programs.

**Your answer / code / interpretation:**

### 20. Choose a visualization for three categorical variables and justify it.

**Your answer / code / interpretation:**

### 21. Write one sentence describing a plot without claiming causation.

**Your answer / code / interpretation:**

## Interpretation and diagnostics

### 22. Find the strongest positive and negative off-diagonal Pearson correlations.

**Your answer / code / interpretation:**

### 23. Find numeric variable pairs with absolute correlation above 0.75. Why is this only a screening rule?

**Your answer / code / interpretation:**

### 24. Compare the study-hours/exam-score relationship within each program.

**Your answer / code / interpretation:**

### 25. Compare pooled and within-program relationships. How can aggregation hide group structure?

**Your answer / code / interpretation:**

### 26. Check whether program and study-group sample sizes are balanced. Why does this matter?

**Your answer / code / interpretation:**

### 27. Name two possible confounders of study hours and exam score.

**Your answer / code / interpretation:**

### 28. Explain how a nonlinear relationship can have low Pearson correlation.

**Your answer / code / interpretation:**

### 29. Explain how correlated predictors affect regression coefficient interpretation.

**Your answer / code / interpretation:**

### 30. Explain how pairwise deletion or complete-case analysis changes the sample.

**Your answer / code / interpretation:**

### 31. List three limitations of interpreting many correlations from one dataset.

**Your answer / code / interpretation:**

## Practical coding

### 32. Write a function that computes a correlation matrix for selected numeric columns and validates column names.

**Your answer / code / interpretation:**

### 33. Build a tidy table of unique correlation pairs without diagonal or mirrored duplicates.

**Your answer / code / interpretation:**

### 34. Calculate group sizes and mean exam scores by program, sorted by mean.

**Your answer / code / interpretation:**

### 35. Calculate within-program correlations and sample sizes for study hours and exam score.

**Your answer / code / interpretation:**

### 36. Create program/result-band counts and row percentages.

**Your answer / code / interpretation:**

### 37. Plot group differences while showing individual observations where practical.

**Your answer / code / interpretation:**

### 38. Explain the difference between a missing value and zero.

**Your answer / code / interpretation:**

### 39. Write three checks to perform before trusting multivariate results.

**Your answer / code / interpretation:**

## Mini-project

### 40. Write a short report using the synthetic student-performance dataset, with the question, variables, methods, and three findings.

**Your answer / code / interpretation:**

### 41. Include two visualizations and one grouped or cross-tabulated summary; explain each.

**Your answer / code / interpretation:**

### 42. Identify one possible interaction and one confounder; state what further data or analysis could help.

**Your answer / code / interpretation:**

### 43. End with a recommendation, limitations, and two follow-up questions. Separate patterns from causal claims.

**Your answer / code / interpretation:**

## Submission checklist

- [ ] State the question and define each variable used.
- [ ] Check missingness, group sizes, and data-quality issues.
- [ ] Label plots clearly and state units where relevant.
- [ ] Interpret correlation direction and magnitude correctly.
- [ ] Compare group-level patterns where appropriate.
- [ ] Avoid unsupported causal claims.
- [ ] Include limitations and follow-up analyses.
