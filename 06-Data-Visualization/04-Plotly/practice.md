# Plotly Practice Exercises

**Instructions:** Complete the tasks in Python using Plotly. Prefer Plotly Express unless a task specifically asks for Graph Objects. Use the built-in datasets where practical so the exercises can run without downloading files.

## Level 1 — Basics

1. Install Plotly and import `plotly.express` as `px`.
2. Load the built-in Iris dataset using `px.data.iris()`. Display the first five rows.
3. Create a scatter plot of `sepal_width` against `sepal_length`.
4. Color the scatter plot by `species` and add a meaningful title.
5. Add `petal_length` and `petal_width` to the hover information.
6. Create a bar chart showing the number of observations for each Iris species. Hint: use `value_counts()` or group by species.
7. Create a line chart using this data:

   | Month | Sales |
   |---|---:|
   | Jan | 120 |
   | Feb | 150 |
   | Mar | 135 |
   | Apr | 180 |
   | May | 210 |
   | Jun | 195 |

8. Add axis titles and a chart title to the sales chart.

## Level 2 — Distributions and Comparisons

9. Load `px.data.tips()`. Create a histogram of `total_bill`.
10. Create a box plot of `total_bill` by `day`, colored by `sex`.
11. Create a scatter plot of `total_bill` vs `tip`, colored by `time`. Add a trendline if your environment supports the required statistics dependency.
12. Create a pie chart showing the share of total bills by day. Aggregate the data before plotting.
13. Use `px.imshow()` to visualize a correlation matrix for numeric columns in the tips dataset. Ensure the axis labels are readable.
14. Create a bar chart of average tip by day and sort the days by average tip.

## Level 3 — Customization and APIs

15. Apply a Plotly template such as `plotly_white` to a chart.
16. Change axis labels using `labels={...}`.
17. Customize the hover tooltip so it displays the customer or category name and relevant numeric values.
18. Use `plotly.graph_objects` to build a bar chart from scratch with at least three categories.
19. Add a second trace to a Graph Objects figure and give each trace a useful legend label.
20. Export a chart to `plotly_chart.html` using `fig.write_html()` and open it in a browser.

## Level 4 — Advanced Visualization

21. Use `px.scatter()` with `facet_col="species"` on the Iris dataset.
22. Create a 3D scatter plot using `sepal_length`, `sepal_width`, and `petal_length`; color points by species.
23. Use `px.data.gapminder()` to create an animated scatter plot of GDP per capita vs life expectancy. Use year as `animation_frame`, continent as color, and population as marker size. Set axis ranges so the chart remains readable.
24. Create a two-panel subplot with Graph Objects and `plotly.subplots.make_subplots()`.
25. Create a chart that communicates a real question from a dataset you choose. Include a title, labeled axes, helpful hover information, and a short written interpretation.

## Reflection Questions

1. What is the difference between Plotly Express and Graph Objects?
2. What does a trace represent?
3. When would you choose a histogram instead of a bar chart?
4. What are the benefits and risks of interactive charts?
5. Why should animation axes often use fixed ranges?
6. When can a pie chart be misleading?
7. What information should be included in a useful hover tooltip?

## Submission Checklist

- [ ] All code runs without errors.
- [ ] Charts have meaningful titles and labels.
- [ ] Categories and units are clear.
- [ ] The selected chart type matches the analytical question.
- [ ] The HTML export opens independently in a browser.
- [ ] Interpretations describe what the chart shows without claiming unsupported causation.
