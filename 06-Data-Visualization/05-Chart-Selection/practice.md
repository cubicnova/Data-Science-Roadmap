# Practice Exercises — Chart Selection

**Instructions:** For selection questions, state the analytical goal, variable types, chosen chart, and one reason for your choice. For coding exercises, use Matplotlib, Seaborn, and Pandas as appropriate. Try the questions before opening `solutions.md`.

## Concepts and question framing

1. Explain why the analytical question should be defined before selecting a chart.
2. A team wants to compare revenue across eight products. Recommend a chart and explain why.
3. A manager wants to show monthly customer growth over two years. Recommend a chart and explain how the x-axis should be ordered.
4. Which chart would you choose to explore the relationship between age and monthly spending? What patterns would you look for?
5. Explain the difference between a chart for a distribution and a chart for a comparison.
6. Give an example where a raw count could be misleading because group sizes differ.
7. Explain when an ECDF can be more useful than a histogram.

## Choose an appropriate chart

8. Choose a chart to rank the ten highest-selling products when product names are long.
9. Choose a chart to compare exam-score distributions across three classes.
10. Choose a chart to show the proportion of orders by payment method when there are four methods.
11. Choose a chart to compare monthly revenue for five regions over one year.
12. Choose a chart to display correlations among ten numeric variables.
13. Choose a chart to compare the distribution of delivery times between standard and express shipping.
14. Choose a chart to show the relationship between advertising spend and sales, with 20,000 observations.
15. Choose a chart to compare percentages across six departments with different sample sizes.
16. Choose a chart to show the frequency of categories in a single column.
17. Choose a chart to compare values across day of week and hour of day.
18. Choose a chart to show uncertainty around average customer satisfaction by product.
19. Explain when a map is appropriate and why a choropleth should often use rates instead of raw counts.

## Evaluate chart choices

20. A pie chart has 18 slices. Explain the problem and suggest an alternative.
21. A line chart connects the values for red, blue, and green product categories. Explain why this may be inappropriate.
22. A bar chart's y-axis begins at 90 and compares values 92 and 98. Explain the risk and recommend an improvement.
23. A scatter plot has thousands of overlapping points. Name two ways to improve it.
24. A report compares 500 incidents in City A with 250 incidents in City B but gives no population context. What should be checked?
25. A 100% stacked bar chart compares category shares across ten groups. Name one strength and one limitation.
26. A heatmap uses a sequential palette for correlations ranging from -1 to +1. Suggest a more suitable color-scale approach.
27. A chart shows a strong correlation and concludes that one variable causes the other. Explain why the conclusion is unsupported.
28. A report uses two y-axes with different scales to make two trends appear similar. Why can this mislead?
29. List five checks to perform before publishing a chart.

## Practical Python

30. Create a bar chart for products `['A', 'B', 'C']` and sales `[40, 65, 50]`, with title and axis labels.
31. Create a line chart for four quarterly revenue values and label the quarters in order.
32. Create a histogram for at least 15 numeric observations and explain how you selected the bins.
33. Create a box plot comparing numeric values across two categories.
34. Create a scatter plot of two numeric variables. Add transparency if points overlap.
35. Use Seaborn to create a count plot for a categorical DataFrame column.
36. Create a heatmap from a correlation matrix and explain why the diagonal values equal 1 for non-constant variables.
37. Make a 100% stacked bar chart from category counts and label the axis as a percentage.

## Mini-project

38. Build a small business dataset containing month, product, revenue, order_value, channel, and ad_spend.
39. Choose and create a chart for revenue trend over time.
40. Choose and create a chart for product performance.
41. Choose and create a chart for order-value distribution.
42. Choose and create a chart for the relationship between advertising spend and revenue.
43. Write a short report with your chart choices, two observations, one limitation per chart, and a note about whether the data supports causal claims.
