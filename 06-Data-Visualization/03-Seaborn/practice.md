# Practice Exercises — Seaborn

**Instructions:** Use Python, Pandas, Seaborn, and Matplotlib. Attempt each task before checking `solutions.md`. Use a local DataFrame so the exercises do not depend on downloaded sample datasets.

## Getting started

1. Install Seaborn and import it as `sns`. Also import Pandas and Matplotlib's pyplot as `plt`.
2. Create a DataFrame with columns `hours=[1,2,3,4,5]`, `score=[50,58,65,72,80]`, and `group=['A','A','B','B','B']`.
3. Explain one difference between an axes-level function such as `scatterplot()` and a figure-level function such as `relplot()`.
4. Create a scatter plot of `hours` versus `score` using the DataFrame.
5. Add a title and descriptive x- and y-axis labels to the plot.

## Relational plots

6. Create a scatter plot and use `hue='group'` to distinguish groups.
7. Use both `hue` and `style` to encode the group variable.
8. Create a line plot for monthly revenue `[100, 120, 115, 140, 155]` across five months.
9. Explain why line charts are appropriate for ordered time values but not always for unrelated categories.
10. Create a `relplot()` scatter chart with `col='group'`.
11. Use `sns.scatterplot()` to explore two numeric columns in a DataFrame of your own.
12. Create a line plot with repeated x values and explain what aggregation may occur by default.
13. Disable the line plot's error bars using the current Seaborn parameter.

## Distribution plots

14. Create a histogram of at least 12 numeric values with eight bins.
15. Add a KDE curve to a histogram.
16. Create a standalone KDE plot and describe one limitation of KDE.
17. Create an ECDF plot and explain what the y-axis represents.
18. Use `displot()` to compare a numeric distribution across two groups.
19. Explain how histogram bin width can change the appearance of a distribution.
20. Compare a histogram and ECDF: give one strength of each.

## Categorical plots

21. Create a box plot to compare scores across two groups.
22. Create a violin plot for the same data and explain how it differs from a box plot.
23. Use a strip plot to show individual observations by group.
24. Use a swarm plot and describe its main benefit and one limitation for large datasets.
25. Create a count plot for a categorical column.
26. Explain the difference between `barplot()` and `countplot()`.
27. Create a categorical plot with `hue='group'` using a suitable dataset.
28. Use `catplot(kind='box')` to compare groups in a figure-level plot.

## Customization and analysis

29. Set a Seaborn theme using `sns.set_theme(style='whitegrid')`.
30. Choose a qualitative palette for categorical groups and explain why it is appropriate.
31. Create a correlation heatmap for at least three numeric columns and annotate the values.
32. Create a regression plot between two numeric variables.
33. Explain why a correlation heatmap does not prove causation.
34. Create a faceted scatter plot using `relplot()` with one column per group.
35. Set a chart title and axis labels on an axes-level Seaborn plot.
36. Name three steps you would take to make a Seaborn chart more readable and accessible.

## Mini-project

37. Create a DataFrame with at least 20 rows containing group, study_hours, score, and attendance columns.
38. Use a box plot and a violin plot to compare scores between groups.
39. Use a scatter plot with hue to examine study hours versus score.
40. Plot the distribution of scores with a histogram and KDE.
41. Create a correlation heatmap for study_hours, score, and attendance.
42. Write three observations grounded in your charts and two limitations. Do not claim causation from association alone.
