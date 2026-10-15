# Practice Exercises — Matplotlib

**Instructions:** Complete the tasks in Python or Jupyter Notebook. Use `examples.ipynb` as a reference, but try to solve each exercise before opening `solutions.md`.

## Getting started

1. Install Matplotlib with pip and import `matplotlib.pyplot` as `plt`.
2. Explain the difference between a Figure and an Axes.
3. Create a line plot for x values `[1, 2, 3, 4]` and y values `[3, 5, 4, 8]`.
4. Add a title, x-axis label, and y-axis label to the chart from Exercise 3.
5. Create a figure with a size of 8 by 4 inches.

## Plot types

6. Create a line chart for monthly temperatures `[22, 24, 23, 27, 29]` and label the months.
7. Create a vertical bar chart for `['Python', 'SQL', 'Excel']` with values `[30, 25, 18]`.
8. Create a horizontal bar chart for the same data.
9. Create a scatter plot for study hours `[1, 2, 3, 4, 5]` and scores `[52, 60, 66, 75, 84]`.
10. Create a histogram for `[12, 15, 15, 18, 19, 21, 22, 22, 25, 28, 30]` using five bins.
11. Create a box plot comparing `[10, 12, 14, 15, 18]` and `[8, 11, 13, 17, 23]`, with group labels.
12. Create a pie chart for values `[50, 30, 20]` with labels and percentage annotations.
13. Explain when a bar chart is a better choice than a pie chart.

## Customization

14. Plot a line with circle markers and a dashed line style.
15. Change a chart's x-axis limits to 0 through 10 and y-axis limits to 0 through 50.
16. Set custom x-ticks at `[0, 2, 4, 6]`.
17. Plot two series on the same axes and add a legend.
18. Add a horizontal grid with low visual emphasis.
19. Use `alpha=0.5` in a scatter plot and explain its effect.
20. Add a horizontal reference line at y = 75 to a chart.
21. Annotate the maximum point in a line chart with text and an arrow.
22. Use `tight_layout()` or constrained layout to prevent overlapping labels.

## Subplots and exporting

23. Create two side-by-side subplots: a bar chart and a line chart.
24. Create a 2-by-2 grid of four different plot types.
25. Give each subplot its own title and label its axes.
26. Add one overall figure title above a group of subplots.
27. Save a chart as `my-chart.png` at 150 DPI with a tight bounding box.
28. Name two vector formats supported by Matplotlib and explain one advantage of vector output.
29. Explain why a logarithmic scale may be useful for values spanning several orders of magnitude.
30. Explain why zero and negative values need special consideration on a logarithmic axis.

## Mini-project

31. Create a Pandas DataFrame with at least six rows of monthly product revenue and plot monthly total revenue.
32. Create a bar chart comparing total revenue by product.
33. Create a histogram from at least 10 order values and describe the distribution.
34. Create a scatter plot of advertising spend versus revenue and state what the plot can and cannot tell you.
35. Combine four charts into a 2-by-2 sales dashboard and add an overall title.
36. Save your dashboard as a PNG and write three data-based observations plus one limitation.
