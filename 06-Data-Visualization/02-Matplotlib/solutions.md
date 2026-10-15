# Solutions — Matplotlib

These are reference solutions. Many exercises can be solved in more than one valid way. The examples favor the object-oriented interface (`fig, ax = plt.subplots()`).

## Getting started

1.
```bash
python -m pip install matplotlib
```
```python
import matplotlib.pyplot as plt
```
2. A **Figure** is the whole canvas; an **Axes** is an individual plotting area within that canvas. One Figure can contain multiple Axes.
3–5.
```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots(figsize=(8, 4))
ax.plot([1, 2, 3, 4], [3, 5, 4, 8], marker="o")
ax.set(title="Example Line Plot", xlabel="X", ylabel="Y")
fig.tight_layout()
plt.show()
```

## Plot types

6.
```python
months = ["Jan", "Feb", "Mar", "Apr", "May"]
temperatures = [22, 24, 23, 27, 29]
fig, ax = plt.subplots()
ax.plot(months, temperatures, marker="o")
ax.set(title="Monthly Temperatures", xlabel="Month", ylabel="Temperature (°C)")
plt.show()
```
7–8.
```python
languages = ["Python", "SQL", "Excel"]
values = [30, 25, 18]

fig, ax = plt.subplots()
ax.bar(languages, values)
ax.set(title="Skills by Count", xlabel="Skill", ylabel="Count")
plt.show()

fig, ax = plt.subplots()
ax.barh(languages, values)
ax.set(title="Skills by Count", xlabel="Count", ylabel="Skill")
plt.show()
```
9.
```python
hours = [1, 2, 3, 4, 5]
scores = [52, 60, 66, 75, 84]
fig, ax = plt.subplots()
ax.scatter(hours, scores)
ax.set(title="Study Hours vs Score", xlabel="Study hours", ylabel="Score")
plt.show()
```
10.
```python
values = [12, 15, 15, 18, 19, 21, 22, 22, 25, 28, 30]
fig, ax = plt.subplots()
ax.hist(values, bins=5, edgecolor="black")
ax.set(title="Value Distribution", xlabel="Value", ylabel="Frequency")
plt.show()
```
11.
```python
fig, ax = plt.subplots()
ax.boxplot([[10, 12, 14, 15, 18], [8, 11, 13, 17, 23]],
           tick_labels=["Group A", "Group B"])
ax.set(title="Group Comparison", ylabel="Value")
plt.show()
```
12.
```python
fig, ax = plt.subplots()
ax.pie([50, 30, 20], labels=["A", "B", "C"], autopct="%1.1f%%")
ax.set_title("Share by Category")
plt.show()
```
13. Bar charts make it easier to compare category values precisely, especially when slices are similar in size or there are many categories. Pie charts are best limited to a few parts of a meaningful whole.

## Customization

14.
```python
fig, ax = plt.subplots()
ax.plot([1, 2, 3, 4], [2, 5, 3, 7], marker="o", linestyle="--")
plt.show()
```
15.
```python
ax.set_xlim(0, 10)
ax.set_ylim(0, 50)
```
16.
```python
ax.set_xticks([0, 2, 4, 6])
```
17.
```python
fig, ax = plt.subplots()
x = [1, 2, 3, 4]
ax.plot(x, [2, 4, 3, 6], marker="o", label="Series A")
ax.plot(x, [1, 3, 4, 5], marker="s", label="Series B")
ax.legend()
plt.show()
```
18. `ax.grid(axis="y", alpha=0.25)` adds subtle horizontal reference lines.
19. `alpha=0.5` makes markers semi-transparent, helping reveal overlapping observations.
20. `ax.axhline(75, linestyle="--", label="Target")`
21. `fig.tight_layout()` adjusts subplot padding. Alternatively, `plt.subplots(..., layout="constrained")` can manage spacing during layout.

## Subplots and exporting

22.
```python
fig, axes = plt.subplots(1, 2, figsize=(10, 4))
axes[0].bar(["A", "B", "C"], [4, 7, 5])
axes[0].set(title="Category Values", xlabel="Category", ylabel="Value")
axes[1].plot([1, 2, 3, 4], [3, 5, 4, 8], marker="o")
axes[1].set(title="Trend", xlabel="Period", ylabel="Value")
fig.tight_layout()
plt.show()
```
23.
```python
fig, axes = plt.subplots(2, 2, figsize=(10, 8))
axes[0, 0].plot([1, 2, 3], [2, 4, 3])
axes[0, 0].set_title("Line")
axes[0, 1].bar(["A", "B", "C"], [3, 5, 2])
axes[0, 1].set_title("Bar")
axes[1, 0].hist([1, 2, 2, 3, 4, 4, 5], bins=4)
axes[1, 0].set_title("Histogram")
axes[1, 1].scatter([1, 2, 3, 4], [4, 3, 5, 7])
axes[1, 1].set_title("Scatter")
fig.tight_layout()
plt.show()
```
24. Use `ax.set(title="...", xlabel="...", ylabel="...")` for each Axes.
25. `fig.suptitle("Sales Performance Dashboard")` adds an overall title. Reserve enough space using constrained layout or adjust spacing if it overlaps.
26.
```python
fig.savefig("my-chart.png", dpi=150, bbox_inches="tight")
```
27. SVG and PDF are vector formats. They can scale without pixelation and are useful for print or later editing.
28. A log scale can make both small and very large positive values visible on the same chart.
29. The ordinary logarithm is undefined for zero and negative values. Such data needs a different scale or a considered transformation, and the choice should be explained.

## Mini-project

30–32. Example:
```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.DataFrame({
    "month": ["Jan", "Jan", "Feb", "Feb", "Mar", "Mar"],
    "product": ["A", "B", "A", "B", "A", "B"],
    "revenue": [120, 90, 140, 110, 135, 125],
    "order_value": [20, 25, 22, 27, 29, 35]
})

monthly = df.groupby("month", sort=False)["revenue"].sum()
by_product = df.groupby("product")["revenue"].sum()

fig, axes = plt.subplots(2, 2, figsize=(11, 8), layout="constrained")
axes[0, 0].plot(monthly.index, monthly.values, marker="o")
axes[0, 0].set(title="Monthly Revenue", xlabel="Month", ylabel="Revenue")

axes[0, 1].bar(by_product.index, by_product.values)
axes[0, 1].set(title="Revenue by Product", xlabel="Product", ylabel="Revenue")

axes[1, 0].hist(df["order_value"], bins=5, edgecolor="black")
axes[1, 0].set(title="Order Value Distribution", xlabel="Order value", ylabel="Frequency")

ad_spend = [2, 3, 4, 5, 6, 7]
observed_revenue = [18, 20, 25, 27, 31, 35]
axes[1, 1].scatter(ad_spend, observed_revenue)
axes[1, 1].set(title="Advertising vs Revenue", xlabel="Ad spend", ylabel="Revenue")

fig.suptitle("Sales Performance Dashboard")
fig.savefig("sales-dashboard.png", dpi=150, bbox_inches="tight")
plt.show()
```
33. The histogram shows the observed order-value distribution. With only six observations, any conclusion about the wider population would be uncertain.
34. A scatter plot can show whether advertising spend and revenue move together in the observed sample. It cannot establish that advertising caused revenue to change; other factors may contribute.
35–36. The dashboard should have readable labels, a clear title, consistent units, and no unnecessary decoration. Write three observations directly supported by the plots and one limitation such as small sample size, synthetic data, or missing context.
