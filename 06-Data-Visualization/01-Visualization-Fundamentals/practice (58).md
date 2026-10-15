# Practice Exercises — Visualization Fundamentals

**Instructions:** Attempt the exercises before checking `solutions.md`. Use Python, Pandas, and Matplotlib where appropriate. The exercises progress from concepts to a small visualization project.

## Foundations

1. Define data visualization and give two reasons it is useful in data analysis.
2. Match each question to a chart: (a) monthly revenue trend, (b) compare sales by product, (c) distribution of customer ages, (d) relationship between height and weight.
3. Explain the difference between categorical and numerical data, with two examples of each.
4. Why should a chart have a descriptive title and labeled axes?
5. Give two examples of how a visualization can mislead its audience.

## Matplotlib basics

6. Import Matplotlib's pyplot module using the conventional alias.
7. Create a bar chart for categories `['Books', 'Games', 'Music']` and values `[12, 19, 8]`.
8. Add the title `Units Sold by Category`, x-axis label `Category`, and y-axis label `Units` to the chart from Exercise 7.
9. Create a line chart for days `['Mon', 'Tue', 'Wed', 'Thu', 'Fri']` and temperatures `[21, 23, 22, 25, 24]`, including markers.
10. Create a histogram from `[12, 15, 15, 18, 19, 21, 21, 21, 25, 28, 30]` using five bins.
11. Create a scatter plot using study hours `[1, 2, 3, 4, 5]` and scores `[50, 56, 65, 70, 82]`.
12. Create a box plot for the values `[10, 12, 13, 15, 18, 20, 35]` and label the y-axis.
13. Set a figure size of 8 by 4 inches for a chart.
14. Add a light horizontal grid to a bar chart and explain why it can help.
15. Use `tight_layout()` before displaying a chart and state its purpose.

## Data and chart selection

16. Create a Pandas DataFrame with columns `product` and `units` using three products, then plot a bar chart.
17. Given daily website visits, explain why a line chart is usually more suitable than a pie chart.
18. Create a line chart for quarterly revenue `[120, 135, 128, 160]` and label the quarters.
19. Create a horizontal bar chart for categories `['North', 'South', 'East']` and values `[42, 35, 51]`.
20. Sort product names by sales before plotting a bar chart. Explain when sorting improves readability.
21. Plot two monthly series, `online=[20, 25, 30, 28]` and `store=[18, 22, 24, 26]`, on the same axes with a legend.
22. Create two side-by-side subplots: one bar chart and one line chart, with a title for each axes.
23. Create a histogram with 10 bins for a numeric list and describe how changing the bin count affects interpretation.
24. Create a scatter plot and set the transparency of markers to 0.6.
25. Plot a box plot comparing scores for two groups and provide meaningful group labels.

## Interpretation and quality

26. Explain why a bar chart that starts its y-axis at 80 can exaggerate differences between values 82 and 88.
27. Describe one situation where a logarithmic scale may be useful and one caution when using it.
28. Explain why a scatter plot showing a positive association does not prove causation.
29. Name three checks you would perform before publishing a chart.
30. Describe two ways to make a chart more accessible to people who may not distinguish colors easily.

## Mini-project and reflection

31. Create a DataFrame containing month, product, and revenue for at least six rows. Plot total revenue by month.
32. Using the same data, create a bar chart comparing total revenue by product.
33. Write three evidence-based observations from your charts and distinguish observations from explanations.
34. Identify one limitation in your sample data and explain how it might affect the conclusions.
35. Choose the best chart for a presentation to a non-technical audience and justify your choice in 3–5 sentences.
