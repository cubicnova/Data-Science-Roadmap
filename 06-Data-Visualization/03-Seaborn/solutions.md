# Solutions — Seaborn

These are reference answers. Open-ended questions can have multiple correct responses. Examples use locally defined DataFrames and current Seaborn APIs.

## Getting started

1.
```bash
python -m pip install seaborn pandas matplotlib jupyter
```
```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
```
2.
```python
df = pd.DataFrame({
    "hours": [1, 2, 3, 4, 5],
    "score": [50, 58, 65, 72, 80],
    "group": ["A", "A", "B", "B", "B"]
})
```
3. Axes-level functions draw on a particular Axes, while figure-level functions manage a figure or grid and often support faceting with `row` and `col`.
4–5.
```python
ax = sns.scatterplot(data=df, x="hours", y="score")
ax.set(title="Study Hours and Score", xlabel="Study hours", ylabel="Score")
plt.tight_layout()
plt.show()
```

## Relational plots

6–7.
```python
sns.scatterplot(data=df, x="hours", y="score", hue="group", style="group")
plt.title("Study Hours and Score by Group")
plt.show()
```
8.
```python
monthly = pd.DataFrame({
    "month": ["Jan", "Feb", "Mar", "Apr", "May"],
    "revenue": [100, 120, 115, 140, 155]
})
sns.lineplot(data=monthly, x="month", y="revenue", marker="o", errorbar=None)
plt.title("Monthly Revenue")
plt.show()
```
9. Time has a natural order, so a line helps communicate continuity and direction. Connecting unrelated categories can imply an order or continuity that does not exist.
10.
```python
sns.relplot(data=df, x="hours", y="score", col="group", kind="scatter")
```
11. Example:
```python
sns.scatterplot(data=df, x="hours", y="score")
plt.show()
```
12. By default, `lineplot()` can aggregate repeated y observations at each x (using an estimator) and display an uncertainty interval. Check the data and function parameters before interpreting the line.
13. Use `errorbar=None` in current Seaborn versions:
```python
sns.lineplot(data=monthly, x="month", y="revenue", errorbar=None)
```

## Distribution plots

14–15.
```python
values = [42, 45, 47, 50, 52, 55, 57, 60, 62, 65, 68, 72, 75, 80]
sns.histplot(values, bins=8, kde=True)
plt.title("Value Distribution")
plt.show()
```
16.
```python
sns.kdeplot(x=values, fill=True)
plt.show()
```
A KDE is a smoothed estimate whose shape depends on bandwidth; it may be misleading for tiny samples or discrete values.
17.
```python
sns.ecdfplot(x=values)
plt.ylabel("Proportion at or below value")
plt.show()
```
18.
```python
dist = pd.DataFrame({
    "value": [12, 15, 18, 20, 21, 23, 25, 27, 30, 31, 35, 38],
    "group": ["A"] * 6 + ["B"] * 6
})
sns.displot(data=dist, x="value", hue="group", kind="hist", bins=6)
```
19. Narrower bins reveal more local detail but can make noise look meaningful; wider bins smooth the distribution but can hide structure.
20. A histogram emphasizes frequency in bins and is useful for seeing shape; an ECDF shows the cumulative proportion at or below every value without a bin-width decision.

## Categorical plots

21.
```python
sns.boxplot(data=df, x="group", y="score")
plt.show()
```
22.
```python
sns.violinplot(data=df, x="group", y="score", inner="quart")
plt.show()
```
A violin plot displays a smoothed density shape, while a box plot emphasizes quartiles, median, and whiskers.
23.
```python
sns.stripplot(data=df, x="group", y="score", jitter=True)
plt.show()
```
24.
```python
sns.swarmplot(data=df, x="group", y="score")
plt.show()
```
Swarm plots reduce point overlap, but can become slow or crowded with large datasets.
25.
```python
sns.countplot(data=df, x="group")
plt.show()
```
26. `barplot()` estimates a summary statistic (typically a mean) and often shows uncertainty; `countplot()` counts observations in each category.
27.
```python
sns.boxplot(data=df, x="group", y="score", hue="group", legend=False)
plt.show()
```
For a plot with a separate grouping variable, choose a dataset with multiple categories in `hue` so the mapping is meaningful.
28.
```python
sns.catplot(data=df, x="group", y="score", kind="box")
```

## Customization and analysis

29.
```python
sns.set_theme(style="whitegrid")
```
30. A qualitative palette is appropriate for categories because it uses distinct colors without implying numeric order.
31.
```python
numeric = pd.DataFrame({
    "hours": [1, 2, 3, 4, 5],
    "score": [50, 58, 65, 72, 80],
    "attendance": [60, 65, 75, 80, 95]
})
sns.heatmap(numeric.corr(), annot=True, vmin=-1, vmax=1, cmap="vlag")
plt.title("Correlation Matrix")
plt.show()
```
32.
```python
sns.regplot(data=df, x="hours", y="score")
plt.show()
```
33. Correlation describes association. Confounding variables, reverse direction, coincidence, and non-linear effects can explain an observed relationship.
34.
```python
sns.relplot(data=df, x="hours", y="score", col="group", kind="scatter")
```
35.
```python
ax = sns.scatterplot(data=df, x="hours", y="score", hue="group")
ax.set(title="Study Hours vs Score", xlabel="Study hours", ylabel="Score")
plt.tight_layout()
plt.show()
```
36. Examples: use a descriptive title and axis labels; choose a readable, accessible palette; avoid overloading hue/style/size; keep comparable axes aligned; show units and context; inspect missing values and outliers.

## Mini-project

37.
```python
import numpy as np
rng = np.random.default_rng(7)
project = pd.DataFrame({
    "group": ["A"] * 10 + ["B"] * 10,
    "study_hours": list(range(1, 11)) + list(range(2, 12)),
    "score": list(rng.integers(50, 80, 10)) + list(rng.integers(60, 95, 10)),
    "attendance": list(rng.integers(65, 96, 20))
})
project.head()
```
38.
```python
sns.boxplot(data=project, x="group", y="score")
plt.show()
sns.violinplot(data=project, x="group", y="score", inner="quart")
plt.show()
```
39.
```python
sns.scatterplot(data=project, x="study_hours", y="score", hue="group")
plt.show()
```
40.
```python
sns.histplot(data=project, x="score", hue="group", bins=8, kde=True)
plt.show()
```
41.
```python
sns.heatmap(project[["study_hours", "score", "attendance"]].corr(),
            annot=True, vmin=-1, vmax=1, cmap="vlag")
plt.show()
```
42. Observations must be derived from the actual generated plots. Possible limitations include small sample size, randomly generated synthetic values, no control for other variables, and no evidence of causal effects.
