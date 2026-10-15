# Solutions — Chart Selection

These are suggested answers. Chart selection depends on the question, data quality, sample size, and audience; alternative choices may also be valid when justified.

## Concepts and question framing

1. The question determines what comparison or pattern matters. Choosing the chart first can emphasize the wrong feature or obscure the answer.
2. A sorted bar chart is a good starting point because it supports direct magnitude comparisons and ranking. A horizontal layout accommodates product names.
3. A line chart with months sorted chronologically. Keep the time axis consistent and label the period and metric.
4. A scatter plot. Look for direction, form, clusters, outliers, and possible nonlinearity. Association does not establish causation.
5. A distribution chart shows the shape and spread of one variable (or compares spread across groups); a comparison chart emphasizes differences between categories or measures.
6. Incident counts across cities with different populations. A rate per 1,000 residents may make the comparison fairer, depending on the question.
7. An ECDF shows the cumulative proportion at or below each value and avoids choosing histogram bins. It is useful for comparing percentiles or threshold-based questions.

## Choose an appropriate chart

8. A sorted horizontal bar chart. It handles long labels and makes ranking clear.
9. Box plots or violin plots, optionally with strip/swarm points. They show distribution differences, not just the mean.
10. A bar chart of counts or proportions. A pie chart may be acceptable for four clear parts of a meaningful whole, but a bar chart is easier to compare precisely.
11. A line chart with one series per region, or small multiples if five lines become crowded. Keep the months in chronological order and axes comparable.
12. A correlation heatmap, with a diverging palette centered at zero. Also inspect scatter plots for important pairs because correlation can hide nonlinear relationships.
13. Box/violin plots or ECDFs by shipping type. Show spread and outliers; do not rely only on averages.
14. A scatter plot with transparency, hexbin plot, or 2D density plot. Choose based on the density pattern and whether individual observations need to remain visible.
15. A 100% stacked bar chart can compare percentage shares, but the denominator and sample sizes should be stated. If exact comparisons matter, use a grouped dot plot or separate rate chart with intervals.
16. A count plot or bar chart of category counts.
17. A heatmap with day on one axis and hour on the other, with a sequential palette for counts. Keep the order of weekdays and hours logical.
18. A point or dot plot with confidence intervals/error bars, with a note explaining the interval type and sample size.
19. A map can be appropriate when geographic location is central to the question. Choropleths often use rates or normalized values so larger populations do not dominate solely because they are larger.

## Evaluate chart choices

20. Eighteen pie slices are hard to compare. Use a sorted bar chart or group small categories into a clearly defined “Other” category if analytically justified.
21. A line implies ordered continuity. Product colors are nominal categories; use a bar chart or dot plot instead unless the categories have a meaningful sequence.
22. Starting at 90 exaggerates the visual difference between 92 and 98. Start the bar axis at zero, or use a dot plot with a clearly marked scale if small differences are the point.
23. Use transparency (`alpha`), a hexbin/density plot, or aggregate points into meaningful bins. Choose based on whether exact points or density is the main goal.
24. Check population size, exposure, reporting rates, time period, and incident definitions. A per-capita rate or other appropriate denominator may be more meaningful than counts.
25. Strength: compares proportions across groups with different totals. Limitation: segments that do not share a common baseline are difficult to compare precisely.
26. Use a diverging palette centered at zero, because positive and negative correlations have different directions and zero is a meaningful midpoint.
27. Correlation alone does not establish direction or causality. Confounders, reverse causation, and coincidence may explain the association.
28. Different axes can make unrelated changes look aligned or amplify one series. Prefer separate aligned panels or a common scale when feasible, and clearly explain any dual axis that is necessary.
29. Check that the chart answers the question; data and denominators are correct; scales and baselines are honest; titles, labels, units, and legend are clear; missing data and uncertainty are handled; colors are accessible; the chart is readable; and conclusions do not exceed the evidence.

## Practical Python

30.
```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots()
ax.bar(["A", "B", "C"], [40, 65, 50])
ax.set(title="Sales by Product", xlabel="Product", ylabel="Sales (units)")
fig.tight_layout()
plt.show()
```
31.
```python
quarters = ["Q1", "Q2", "Q3", "Q4"]
revenue = [120, 135, 128, 160]
fig, ax = plt.subplots()
ax.plot(quarters, revenue, marker="o")
ax.set(title="Quarterly Revenue", xlabel="Quarter", ylabel="Revenue")
plt.show()
```
32.
```python
fig, ax = plt.subplots()
ax.hist(values, bins="auto", edgecolor="black")
ax.set(title="Distribution of Values", xlabel="Value", ylabel="Frequency")
plt.show()
```
Explain the bin rule or choice; try alternatives to see whether the overall shape is robust.
33.
```python
import seaborn as sns
sns.boxplot(data=df, x="category", y="value")
plt.show()
```
34.
```python
fig, ax = plt.subplots()
ax.scatter(x_values, y_values, alpha=0.5)
ax.set(xlabel="X variable", ylabel="Y variable", title="Relationship between X and Y")
plt.show()
```
35.
```python
sns.countplot(data=df, x="category")
plt.show()
```
36.
```python
correlation = df.select_dtypes(include="number").corr()
sns.heatmap(correlation, annot=True, vmin=-1, vmax=1, cmap="vlag")
plt.show()
```
For a non-constant variable, its correlation with itself is 1. Constant columns may produce undefined correlations.
37. Example using Pandas:
```python
counts = pd.crosstab(df["group"], df["channel"], normalize="index") * 100
ax = counts.plot(kind="bar", stacked=True)
ax.set(ylabel="Orders (%)", title="Channel Mix by Group")
ax.legend(title="Channel", bbox_to_anchor=(1.02, 1))
plt.tight_layout()
plt.show()
```

## Mini-project

38–43. One possible dataset and chart plan:
```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

df = pd.DataFrame({
    "month": ["Jan", "Jan", "Feb", "Feb", "Mar", "Mar", "Apr", "Apr"],
    "product": ["A", "B"] * 4,
    "revenue": [120, 90, 140, 110, 135, 125, 160, 130],
    "order_value": [20, 25, 22, 27, 24, 29, 28, 35],
    "channel": ["Web", "Store"] * 4,
    "ad_spend": [10, 8, 12, 9, 11, 10, 15, 12]
})

monthly = df.groupby("month", sort=False)["revenue"].sum()
monthly.plot(marker="o", title="Monthly Revenue")
plt.ylabel("Revenue")
plt.show()

sns.barplot(data=df, x="product", y="revenue", estimator="sum", errorbar=None)
plt.title("Revenue by Product")
plt.show()

sns.histplot(data=df, x="order_value", bins=5)
plt.title("Order Value Distribution")
plt.show()

sns.scatterplot(data=df, x="ad_spend", y="revenue", hue="channel")
plt.title("Advertising Spend vs Revenue")
plt.show()
```
A valid report should justify each chart, describe only visible patterns, and mention limitations such as a small sample, a short time range, synthetic data, missing context, or confounding factors. A scatter plot alone cannot establish that advertising caused revenue to change.
