# Solutions — Visualization Fundamentals

These are reference answers, not the only valid solutions. For open-ended questions, use the explanations as a checklist and support conclusions with the displayed data.

## Foundations

1. Data visualization represents data graphically. It helps reveal patterns and communicate comparisons or trends efficiently.
2. (a) Line chart, (b) bar chart, (c) histogram or box plot, (d) scatter plot.
3. Categorical data describes groups or labels, such as product type or city. Numerical data represents measured or counted quantities, such as revenue or age.
4. Titles explain the chart's subject; axis labels identify the variables and units so values can be interpreted correctly.
5. Examples: truncating a bar chart's axis to exaggerate differences; using a distorted 3D chart; omitting a relevant time period; using inconsistent scales.

## Matplotlib basics

6. `import matplotlib.pyplot as plt`
7–8.
```python
import matplotlib.pyplot as plt

categories = ["Books", "Games", "Music"]
values = [12, 19, 8]
fig, ax = plt.subplots()
ax.bar(categories, values)
ax.set(title="Units Sold by Category", xlabel="Category", ylabel="Units")
fig.tight_layout()
plt.show()
```
9.
```python
days = ["Mon", "Tue", "Wed", "Thu", "Fri"]
temperatures = [21, 23, 22, 25, 24]
fig, ax = plt.subplots()
ax.plot(days, temperatures, marker="o")
ax.set(title="Daily Temperature", xlabel="Day", ylabel="Temperature (°C)")
fig.tight_layout()
plt.show()
```
10.
```python
values = [12, 15, 15, 18, 19, 21, 21, 21, 25, 28, 30]
fig, ax = plt.subplots()
ax.hist(values, bins=5, edgecolor="black")
ax.set(xlabel="Value", ylabel="Frequency", title="Value Distribution")
plt.show()
```
11.
```python
hours = [1, 2, 3, 4, 5]
scores = [50, 56, 65, 70, 82]
fig, ax = plt.subplots()
ax.scatter(hours, scores)
ax.set(xlabel="Study hours", ylabel="Score", title="Study Hours vs Score")
plt.show()
```
12.
```python
values = [10, 12, 13, 15, 18, 20, 35]
fig, ax = plt.subplots()
ax.boxplot(values)
ax.set(ylabel="Value", title="Value Summary")
plt.show()
```
13. `fig, ax = plt.subplots(figsize=(8, 4))`
14. `ax.grid(axis="y", alpha=0.25)` adds subtle horizontal reference lines that help compare bar heights.
15. `fig.tight_layout()` adjusts spacing to reduce overlap or clipping of labels and titles.

## Data and chart selection

16.
```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.DataFrame({"product": ["A", "B", "C"], "units": [12, 19, 8]})
ax = df.plot(x="product", y="units", kind="bar", legend=False)
ax.set(title="Units by Product", xlabel="Product", ylabel="Units")
plt.tight_layout()
plt.show()
```
17. Time is ordered, so a line chart makes direction and changes across consecutive days easy to see.
18.
```python
quarters = ["Q1", "Q2", "Q3", "Q4"]
revenue = [120, 135, 128, 160]
fig, ax = plt.subplots()
ax.plot(quarters, revenue, marker="o")
ax.set(title="Quarterly Revenue", xlabel="Quarter", ylabel="Revenue")
plt.show()
```
19.
```python
fig, ax = plt.subplots()
ax.barh(["North", "South", "East"], [42, 35, 51])
ax.set(title="Values by Region", xlabel="Value", ylabel="Region")
plt.show()
```
20.
```python
sales = {"Alpha": 24, "Beta": 35, "Gamma": 19}
ordered = sorted(sales.items(), key=lambda item: item[1], reverse=True)
labels, values = zip(*ordered)
fig, ax = plt.subplots()
ax.bar(labels, values)
plt.show()
```
Sorting helps viewers find high and low values quickly, especially when there are many categories.
21.
```python
months = ["Jan", "Feb", "Mar", "Apr"]
online = [20, 25, 30, 28]
store = [18, 22, 24, 26]
fig, ax = plt.subplots()
ax.plot(months, online, marker="o", label="Online")
ax.plot(months, store, marker="s", label="Store")
ax.set(xlabel="Month", ylabel="Orders", title="Orders by Channel")
ax.legend()
plt.show()
```
22.
```python
fig, axes = plt.subplots(1, 2, figsize=(10, 4))
axes[0].bar(["A", "B", "C"], [4, 7, 5])
axes[0].set_title("Category Values")
axes[1].plot([1, 2, 3, 4], [3, 5, 4, 8], marker="o")
axes[1].set_title("Trend")
fig.tight_layout()
plt.show()
```
23. More bins show finer detail but may make random fluctuations look meaningful; fewer bins smooth the shape but may hide structure. Try sensible alternatives and state the bin choice.
24. `ax.scatter(x, y, alpha=0.6)` makes overlapping points easier to see.
25.
```python
group_a = [62, 65, 68, 70, 72, 75, 78]
group_b = [55, 60, 63, 67, 71, 80, 92]
fig, ax = plt.subplots()
ax.boxplot([group_a, group_b], tick_labels=["Group A", "Group B"])
ax.set(ylabel="Score", title="Scores by Group")
plt.show()
```

## Interpretation and quality

26. The shortened axis makes a small absolute difference occupy a large portion of the visible chart. Bar charts should generally start at zero when length represents magnitude.
27. A logarithmic scale can show values spanning several orders of magnitude, such as population across cities. It changes how distances are interpreted; label it clearly and avoid it when the audience may misread it.
28. Other variables may influence both values, the relationship may be coincidental, or the direction of influence may be unclear. Correlation describes association, not cause.
29. Check that data and units are correct; choose a suitable chart and honest scale; verify titles, labels, legend, and readability; inspect missing values and outliers; include context and limitations.
30. Use a colorblind-friendly palette, plus markers or line styles; add direct labels or patterns; maintain adequate contrast and readable text.

## Mini-project and reflection

31–32. Example:
```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.DataFrame({
    "month": ["Jan", "Jan", "Feb", "Feb", "Mar", "Mar"],
    "product": ["A", "B", "A", "B", "A", "B"],
    "revenue": [120, 90, 140, 110, 135, 125]
})

monthly = df.groupby("month", sort=False)["revenue"].sum()
fig, ax = plt.subplots()
ax.plot(monthly.index, monthly.values, marker="o")
ax.set(title="Total Revenue by Month", xlabel="Month", ylabel="Revenue")
plt.show()

by_product = df.groupby("product")["revenue"].sum().sort_values(ascending=False)
fig, ax = plt.subplots()
ax.bar(by_product.index, by_product.values)
ax.set(title="Revenue by Product", xlabel="Product", ylabel="Revenue")
plt.show()
```
33. Answers depend on the plotted results. State the measurable pattern first (for example, “revenue increased from January to February”), then distinguish any possible explanation as a hypothesis.
34. A small sample, short time window, missing observations, or synthetic data may not represent the real population and can make trends unstable.
35. There is no single correct choice. For a non-technical audience, prefer a simple chart that directly answers the question, has clear labels and units, and highlights the key comparison without clutter. Explain why that chart fits the message.
