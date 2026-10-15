# Visualization Fundamentals

**Module:** `06-Data-Visualization/01-Visualization-Fundamentals`  
**Level:** Beginner  
**Focus:** Learn how to choose, build, interpret, and improve charts with Python.

## Overview

Data visualization turns data into visual forms that help people identify patterns, compare values, understand distributions, and communicate findings. A good chart is accurate, readable, appropriate for the question, and honest about the underlying data.

This module introduces visualization principles before moving into more specialized chart types and libraries.

## Learning objectives

By the end of this module, you should be able to:

- Explain why visualization is useful in data analysis.
- Identify categorical, numerical, temporal, and relational data.
- Choose an appropriate chart for a question.
- Create basic charts with Matplotlib and Pandas.
- Add titles, axis labels, legends, annotations, and readable ticks.
- Recognize misleading scales, clutter, and inappropriate chart choices.
- Interpret visual patterns without claiming more than the data supports.

## Prerequisites

- Basic Python: variables, lists, dictionaries, functions.
- Basic Pandas: DataFrames and selecting columns.
- Basic statistics: mean, median, minimum, maximum, and counts.

## Setup

Install the libraries in your Python environment:

```bash
python -m pip install pandas matplotlib jupyter
```

Launch Jupyter:

```bash
jupyter notebook
```

Open `examples.ipynb` and run the cells from top to bottom. The examples use small, self-contained datasets, so no external data download is needed.

## 1. Why visualize data?

Tables are useful for exact values, but charts make some patterns easier to notice. Visualization can help answer questions such as:

- Which category has the highest value?
- How did a metric change over time?
- Is a variable skewed or spread out?
- Are two numerical variables associated?
- Are there unusual values that deserve investigation?

A chart helps explore and communicate data; it does not by itself prove causation.

## 2. Understand the data and question

Before plotting, identify the question and the type of data.

| Question | Typical data | Useful starting chart |
|---|---|---|
| Compare categories | Category + numeric measure | Bar chart |
| Track change over time | Date/time + numeric measure | Line chart |
| Examine a distribution | One numeric variable | Histogram or box plot |
| Explore a relationship | Two numeric variables | Scatter plot |
| Compare parts of a whole | A small number of mutually exclusive parts | Bar chart; sometimes pie chart |
| Compare two groups' distributions | Category + numeric variable | Box plot or violin plot |

Chart choice depends on the task, number of categories, data quality, and audience.

## 3. Core chart types

### Bar chart

Use bars to compare values across categories. Start the value axis at zero when bar length encodes magnitude. Sort categories when that makes comparison easier.

```python
import matplotlib.pyplot as plt

categories = ["A", "B", "C"]
values = [24, 35, 19]

fig, ax = plt.subplots()
ax.bar(categories, values)
ax.set(title="Values by category", xlabel="Category", ylabel="Value")
plt.show()
```

### Line chart

Use a line chart for ordered data, especially time series. Sort dates chronologically and avoid connecting unrelated categories.

```python
months = ["Jan", "Feb", "Mar", "Apr"]
sales = [120, 135, 128, 160]

fig, ax = plt.subplots()
ax.plot(months, sales, marker="o")
ax.set(title="Monthly sales", xlabel="Month", ylabel="Sales")
plt.show()
```

### Histogram

A histogram groups numerical observations into bins to show a distribution. The appearance can change with bin width, so choose bins thoughtfully.

```python
scores = [55, 61, 64, 66, 68, 70, 72, 73, 75, 77, 80, 84, 91]
fig, ax = plt.subplots()
ax.hist(scores, bins=5, edgecolor="black")
ax.set(title="Score distribution", xlabel="Score", ylabel="Frequency")
plt.show()
```

### Scatter plot

Use a scatter plot to inspect the relationship between two numerical variables. A visible association does not establish causation.

```python
study_hours = [1, 2, 2.5, 3, 4, 5, 6]
scores = [52, 57, 61, 65, 72, 79, 85]
fig, ax = plt.subplots()
ax.scatter(study_hours, scores)
ax.set(title="Study time and score", xlabel="Study hours", ylabel="Score")
plt.show()
```

### Box plot

A box plot summarizes a distribution using the median, quartiles, and whiskers. Confirm the plotting library's whisker convention before interpreting outliers.

```python
group_a = [62, 65, 68, 70, 72, 75, 78]
group_b = [55, 60, 63, 67, 71, 80, 92]
fig, ax = plt.subplots()
ax.boxplot([group_a, group_b], tick_labels=["Group A", "Group B"])
ax.set(title="Scores by group", ylabel="Score")
plt.show()
```

## 4. Build a chart with Matplotlib

A typical workflow:

1. Prepare and validate the data.
2. Create a figure and axes.
3. Plot the data.
4. Add a descriptive title and axis labels.
5. Add a legend only when it clarifies multiple series.
6. Improve ticks, grid lines, and layout.
7. Check that the chart is accurate and readable.
8. Interpret the result in plain language.

```python
fig, ax = plt.subplots(figsize=(7, 4))
ax.bar(categories, values)
ax.set_title("Values by category")
ax.set_xlabel("Category")
ax.set_ylabel("Value")
ax.grid(axis="y", alpha=0.25)
fig.tight_layout()
plt.show()
```

Prefer the object-oriented `fig, ax = plt.subplots()` style for reusable, maintainable charts.

## 5. Pandas plotting

Pandas provides convenient plotting methods backed by Matplotlib.

```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.DataFrame({"month": ["Jan", "Feb", "Mar"], "orders": [18, 24, 21]})
df.plot(x="month", y="orders", kind="bar", legend=False)
plt.title("Orders by month")
plt.xlabel("Month")
plt.ylabel("Orders")
plt.tight_layout()
plt.show()
```

Pandas plotting is useful for quick exploration; use Matplotlib axes when you need more detailed customization.

## 6. Design principles

- **Match the chart to the question.** Do not use a chart just because it looks attractive.
- **Make labels informative.** Use clear titles, units, and axis labels.
- **Keep scales honest.** Truncated axes can exaggerate differences, especially in bar charts.
- **Reduce clutter.** Avoid unnecessary 3D effects, excessive grid lines, and decorative elements.
- **Use color with purpose.** Color should encode groups or draw attention to a meaningful point, not distract.
- **Support accessibility.** Do not rely on color alone; use labels, markers, line styles, and adequate contrast.
- **Show context.** Include time ranges, units, sample sizes, or data sources when they matter.
- **Respect uncertainty.** Explain missing data, small samples, and limitations.
- **Avoid overclaiming.** Correlation is not proof of causation.
- **Consider the audience.** A chart for quick exploration may differ from one in a report.

## 7. Common mistakes

| Mistake | Why it is a problem | Improvement |
|---|---|---|
| Wrong chart type | Makes the question harder to answer | Start with the analytical question |
| Missing units | Values become ambiguous | Label axes with units |
| Overloaded legend | Readers struggle to compare series | Reduce series or label directly |
| Too many categories | Labels become unreadable | Sort, group, or show a focused subset |
| Inconsistent scales | Comparisons can be misleading | Use comparable axes for comparable data |
| Treating correlation as causation | Implies an unsupported explanation | Describe association and investigate further |
| Ignoring missing values | Can distort the displayed data | Inspect and document missingness |

## 8. Mini-project

Use the sample sales dataset in `examples.ipynb` to:

1. Compare sales across products with a bar chart.
2. Show monthly revenue with a line chart.
3. Inspect the distribution of order values with a histogram.
4. Explore the relationship between advertising spend and revenue with a scatter plot.
5. Write three observations and one limitation.

## Files in this module

- [`README.md`](README.md) — concepts, chart selection, and best practices.
- [`examples.ipynb`](examples.ipynb) — runnable examples using Matplotlib and Pandas.
- [`practice.md`](practice.md) — 35 progressive exercises.
- [`solutions.md`](solutions.md) — reference answers and code hints.

## Further reading

- [Matplotlib documentation](https://matplotlib.org/stable/)
- [Pandas visualization guide](https://pandas.pydata.org/docs/user_guide/visualization.html)
- [Data-to-Viz chart selection guide](https://www.data-to-viz.com/)

## Completion checklist

- [ ] I can match a chart to a question.
- [ ] I can create a labeled bar, line, histogram, and scatter plot.
- [ ] I can improve readability with titles, labels, legends, and layout.
- [ ] I can identify common misleading visualization practices.
- [ ] I completed the practice exercises and mini-project.
