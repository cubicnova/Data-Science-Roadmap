# Matplotlib Cheat Sheet

A practical, beginner-friendly reference for data visualization in Python. This cheat sheet covers essential Matplotlib functions, chart types, styling, subplot layouts, saving figures, and common plotting patterns used in data analysis, statistics, and machine learning.

**Repository:** [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap)

---

## Table of Contents

1. [Introduction to Matplotlib](#1-introduction-to-matplotlib)
2. [Installation and Imports](#2-installation-and-imports)
3. [Matplotlib Figure Structure](#3-matplotlib-figure-structure)
4. [Basic Plotting](#4-basic-plotting)
5. [Common Chart Types](#5-common-chart-types)
6. [Titles, Labels, and Legends](#6-titles-labels-and-legends)
7. [Colors, Markers, and Line Styles](#7-colors-markers-and-line-styles)
8. [Axis Limits, Ticks, and Scales](#8-axis-limits-ticks-and-scales)
9. [Grid, Annotations, and Text](#9-grid-annotations-and-text)
10. [Subplots and Multiple Charts](#10-subplots-and-multiple-charts)
11. [Working with NumPy and Pandas](#11-working-with-numpy-and-pandas)
12. [Statistical Visualizations](#12-statistical-visualizations)
13. [Customizing Styles](#13-customizing-styles)
14. [Saving and Exporting Figures](#14-saving-and-exporting-figures)
15. [Useful Functions Reference](#15-useful-functions-reference)
16. [Common Plotting Patterns](#16-common-plotting-patterns)
17. [Best Practices](#17-best-practices)
18. [Common Errors and Solutions](#18-common-errors-and-solutions)
19. [Practice Exercises](#19-practice-exercises)
20. [Quick Revision](#20-quick-revision)
21. [Official Resources](#21-official-resources)

---

## 1. Introduction to Matplotlib

Matplotlib is a Python visualization library used to create static, animated, and interactive charts.

### Common use cases

- Exploring datasets during exploratory data analysis (EDA).
- Visualizing statistical distributions and relationships.
- Comparing categories and tracking trends.
- Evaluating machine learning models.
- Creating publication-quality charts and reports.
- Visualizing time-series and scientific data.

### Why use Matplotlib?

- Extensive chart customization.
- Integration with NumPy and Pandas.
- Support for multiple chart types.
- Export to PNG, JPEG, SVG, PDF, and other formats.
- Compatibility with Jupyter notebooks and Python scripts.

### Recommended plotting interface

Matplotlib provides two common interfaces:

| Interface | Example | Best suited for |
|---|---|---|
| Pyplot | `plt.plot(x, y)` | Quick exploratory plotting |
| Object-oriented | `ax.plot(x, y)` | Reusable code and complex figures |

**Recommendation:** Prefer `fig, ax = plt.subplots()` and the `ax` methods for reusable projects.

---

## 2. Installation and Imports

### Install Matplotlib

```bash
pip install matplotlib
```

Install NumPy and Pandas for data analysis:

```bash
pip install numpy pandas
```

### Import libraries

```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
```

### Your first plot

```python
import matplotlib.pyplot as plt

x = [1, 2, 3, 4, 5]
y = [10, 20, 15, 30, 25]

fig, ax = plt.subplots()

ax.plot(x, y)
ax.set_title("My First Plot")
ax.set_xlabel("X Values")
ax.set_ylabel("Y Values")

plt.show()
```

**Remember:** `plt.show()` displays the figure in many Python script environments. Jupyter notebooks often display figures automatically.

---

## 3. Matplotlib Figure Structure

A Matplotlib visualization has several important components.

| Component | Meaning |
|---|---|
| Figure | The entire visualization canvas |
| Axes | The area where data is plotted |
| Axis | The x-axis or y-axis that controls ticks and scale |
| Artist | A drawable element, such as a line, text, or legend |
| Legend | Explains the meaning of plotted series |
| Title | Describes the chart |
| Labels | Identify the meaning and units of each axis |

### Create a figure and axes

```python
fig, ax = plt.subplots(figsize=(8, 5))

ax.plot([1, 2, 3], [10, 20, 15])

ax.set_title("Sample Chart")
ax.set_xlabel("X")
ax.set_ylabel("Y")

plt.show()
```

### Common figure parameters

```python
fig, ax = plt.subplots(
    figsize=(10, 6),
    dpi=100,
    layout="constrained"
)
```

- `figsize`: figure width and height in inches.
- `dpi`: dots per inch, affecting raster rendering resolution.
- `layout="constrained"`: helps prevent labels and titles from overlapping.

---

## 4. Basic Plotting

### 4.1 Line plot

Use a line plot to show trends, especially over time or an ordered sequence.

```python
x = [1, 2, 3, 4, 5]
y = [2, 4, 3, 7, 6]

fig, ax = plt.subplots()
ax.plot(x, y)

plt.show()
```

### 4.2 Plot multiple lines

```python
months = [1, 2, 3, 4, 5]

product_a = [20, 25, 23, 30, 35]
product_b = [15, 18, 22, 25, 28]

fig, ax = plt.subplots()

ax.plot(months, product_a, label="Product A")
ax.plot(months, product_b, label="Product B")

ax.set_xlabel("Month")
ax.set_ylabel("Sales")
ax.legend()

plt.show()
```

### 4.3 Plot with automatically generated x-values

```python
values = [10, 15, 12, 20, 18]

fig, ax = plt.subplots()
ax.plot(values)

plt.show()
```

When only y-values are provided, Matplotlib generates x-values starting at zero.

### 4.4 Plot mathematical functions

```python
x = np.linspace(0, 2 * np.pi, 200)
y = np.sin(x)

fig, ax = plt.subplots()

ax.plot(x, y)
ax.set_xlabel("Radians")
ax.set_ylabel("sin(x)")
ax.set_title("Sine Function")

plt.show()
```

---

## 5. Common Chart Types

### 5.1 Bar chart

Use a bar chart to compare categories.

```python
categories = ["Python", "SQL", "ML", "Statistics"]
scores = [85, 90, 78, 88]

fig, ax = plt.subplots()

ax.bar(categories, scores)

ax.set_title("Subject Scores")
ax.set_ylabel("Score")
ax.set_ylim(0, 100)

plt.show()
```

**Horizontal bar chart**

```python
fig, ax = plt.subplots()

ax.barh(categories, scores)
ax.set_xlabel("Score")

plt.show()
```

### 5.2 Scatter plot

Use a scatter plot to explore relationships between two numerical variables.

```python
hours = [1, 2, 3, 4, 5, 6]
scores = [40, 45, 55, 60, 75, 85]

fig, ax = plt.subplots()

ax.scatter(hours, scores)
ax.set_xlabel("Study Hours")
ax.set_ylabel("Exam Score")

plt.show()
```

### 5.3 Histogram

Use a histogram to examine the distribution of a numerical variable.

```python
data = np.random.normal(
    loc=50,
    scale=10,
    size=1000
)

fig, ax = plt.subplots()

ax.hist(data, bins=20, edgecolor="black")
ax.set_xlabel("Value")
ax.set_ylabel("Frequency")
ax.set_title("Data Distribution")

plt.show()
```

Important parameters:

- `bins`: number of bins or explicit bin boundaries.
- `density=True`: normalize the histogram so its total area is approximately one.
- `edgecolor`: color of bin borders.
- `alpha`: transparency.

### 5.4 Pie chart

Use a pie chart to display proportions when there are only a few categories.

```python
labels = ["Python", "SQL", "Machine Learning", "Other"]
sizes = [40, 25, 25, 10]

fig, ax = plt.subplots()

ax.pie(
    sizes,
    labels=labels,
    autopct="%1.1f%%",
    startangle=90
)

ax.set_title("Skill Distribution")
plt.show()
```

Avoid pie charts with too many categories. Bar charts are usually easier to compare accurately.

### 5.5 Box plot

Use a box plot to summarize a numerical distribution and identify potential outliers.

```python
scores = [55, 60, 62, 65, 68, 70, 72, 75, 80, 100]

fig, ax = plt.subplots()

ax.boxplot(scores)
ax.set_ylabel("Score")
ax.set_title("Score Distribution")

plt.show()
```

The box represents the interquartile range, while the center line indicates the median. Whiskers and individual outlier markers follow the plotting function's configured rules.

### 5.6 Area chart

```python
x = [1, 2, 3, 4, 5]
y = [10, 15, 12, 20, 25]

fig, ax = plt.subplots()

ax.fill_between(x, y, alpha=0.3)
ax.plot(x, y)

plt.show()
```

### 5.7 Step plot

```python
time = [0, 1, 2, 3, 4]
state = [0, 1, 1, 0, 1]

fig, ax = plt.subplots()

ax.step(time, state, where="post")
ax.set_xlabel("Time")
ax.set_ylabel("State")

plt.show()
```

Useful for discrete changes, event sequences, and state transitions.

### 5.8 Error bars

```python
x = [1, 2, 3, 4]
means = [10, 12, 15, 14]
errors = [1, 2, 1.5, 1]

fig, ax = plt.subplots()

ax.errorbar(
    x,
    means,
    yerr=errors,
    fmt="o-",
    capsize=4
)

plt.show()
```

Use error bars to communicate uncertainty or variability when their statistical meaning is clearly defined.

### 5.9 Heatmap

Use `imshow()` to visualize a two-dimensional numerical matrix.

```python
data = np.array([
    [10, 20, 30],
    [15, 25, 35],
    [20, 30, 40]
])

fig, ax = plt.subplots()

image = ax.imshow(data, cmap="viridis")
fig.colorbar(image, ax=ax, label="Value")

ax.set_title("Heatmap")

plt.show()
```

---

## 6. Titles, Labels, and Legends

### Add a title

```python
ax.set_title("Monthly Revenue")
```

### Add axis labels

```python
ax.set_xlabel("Month")
ax.set_ylabel("Revenue (₹)")
```

### Add a legend

```python
ax.plot([1, 2, 3], [10, 20, 30], label="Revenue")
ax.legend()
```

### Customize title and labels

```python
ax.set_title(
    "Monthly Revenue",
    fontsize=16,
    fontweight="bold"
)

ax.set_xlabel("Month", fontsize=12)
ax.set_ylabel("Revenue (₹)", fontsize=12)
```

### Position a legend

```python
ax.legend(loc="upper left")
```

Common locations include:

- `"upper right"`
- `"upper left"`
- `"lower right"`
- `"lower left"`
- `"center"`
- `"best"`

### Set axis labels with units

```python
ax.set_xlabel("Time (seconds)")
ax.set_ylabel("Temperature (°C)")
```

Including units makes a chart easier to interpret.

---

## 7. Colors, Markers, and Line Styles

### Common line styles

| Style | Parameter |
|---|---|
| Solid | `linestyle="-"` |
| Dashed | `linestyle="--"` |
| Dotted | `linestyle=":"` |
| Dash-dot | `linestyle="-."` |

### Common markers

| Marker | Meaning |
|---|---|
| `"o"` | Circle |
| `"s"` | Square |
| `"^"` | Upward triangle |
| `"x"` | Cross |
| `"+"` | Plus |
| `"D"` | Diamond |
| `"*"` | Star |

### Customize a line

```python
fig, ax = plt.subplots()

ax.plot(
    [1, 2, 3, 4],
    [10, 20, 15, 25],
    color="blue",
    linestyle="--",
    marker="o",
    linewidth=2,
    markersize=7,
    label="Sales"
)

ax.legend()
plt.show()
```

### Customize a scatter plot

```python
fig, ax = plt.subplots()

ax.scatter(
    [1, 2, 3, 4],
    [10, 15, 13, 20],
    s=100,
    c=[10, 20, 30, 40],
    cmap="viridis",
    alpha=0.8,
    edgecolors="black"
)

plt.show()
```

Here, `s` controls marker area and `c` supplies the values used by the colormap.

### Transparency

```python
ax.plot(x, y, alpha=0.5)
```

`alpha=0` is fully transparent, and `alpha=1` is fully opaque.

### Common colormaps

Examples include:

- `"viridis"`
- `"plasma"`
- `"inferno"`
- `"magma"`
- `"cividis"`
- `"coolwarm"`

Use sequential colormaps for ordered values, diverging colormaps for values around a meaningful midpoint, and qualitative colors for unrelated categories.

---

## 8. Axis Limits, Ticks, and Scales

### Set axis limits

```python
ax.set_xlim(0, 10)
ax.set_ylim(0, 100)
```

### Set tick positions

```python
ax.set_xticks([0, 2, 4, 6, 8, 10])
ax.set_yticks([0, 20, 40, 60, 80, 100])
```

### Set custom tick labels

```python
ax.set_xticks(
    [1, 2, 3],
    ["Jan", "Feb", "Mar"]
)
```

Use positions and labels of equal length. Avoid assigning labels without defining their corresponding tick positions.

### Rotate tick labels

```python
ax.tick_params(axis="x", labelrotation=45)
```

### Logarithmic scale

Useful when values span several orders of magnitude.

```python
x = [1, 2, 3, 4, 5]
y = [10, 100, 1000, 10000, 100000]

fig, ax = plt.subplots()

ax.plot(x, y)
ax.set_yscale("log")

plt.show()
```

A standard logarithmic scale requires positive values.

Other useful options:

```python
ax.set_xscale("log")
ax.set_yscale("linear")
```

For logarithmic plots, explain the scale clearly because equal visual distances represent multiplicative rather than additive changes.

---

## 9. Grid, Annotations, and Text

### Add a grid

```python
ax.grid(True)
```

Customize the grid:

```python
ax.grid(
    True,
    linestyle="--",
    alpha=0.4
)
```

### Add text to a chart

```python
ax.text(
    2,
    20,
    "Important Point",
    fontsize=10
)
```

The coordinates above are data coordinates by default.

### Annotate a data point

```python
x = [1, 2, 3, 4]
y = [10, 15, 30, 25]

fig, ax = plt.subplots()
ax.plot(x, y, marker="o")

ax.annotate(
    "Peak",
    xy=(3, 30),
    xytext=(3.3, 35),
    arrowprops={"arrowstyle": "->"}
)

plt.show()
```

Annotations are useful for highlighting important events, outliers, peaks, or business milestones.

### Add a horizontal or vertical reference line

```python
ax.axhline(y=20, linestyle="--", label="Target")
ax.axvline(x=3, linestyle=":", label="Deadline")
ax.legend()
```

---

## 10. Subplots and Multiple Charts

### Create a 1 × 2 layout

```python
fig, axes = plt.subplots(1, 2, figsize=(10, 4))

axes[0].plot([1, 2, 3], [10, 20, 15])
axes[0].set_title("Line Plot")

axes[1].bar(["A", "B", "C"], [10, 20, 15])
axes[1].set_title("Bar Chart")

plt.show()
```

### Create a 2 × 2 layout

```python
fig, axes = plt.subplots(2, 2, figsize=(10, 8))

axes[0, 0].plot([1, 2, 3], [2, 4, 3])
axes[0, 0].set_title("Line")

axes[0, 1].scatter([1, 2, 3], [3, 1, 4])
axes[0, 1].set_title("Scatter")

axes[1, 0].bar(["A", "B"], [10, 20])
axes[1, 0].set_title("Bar")

axes[1, 1].hist([1, 2, 2, 3, 3, 3, 4], bins=4)
axes[1, 1].set_title("Histogram")

fig.suptitle("Visualization Dashboard")
plt.show()
```

### Share axes

```python
fig, axes = plt.subplots(
    2, 1,
    sharex=True,
    sharey=True
)
```

Shared axes help compare multiple datasets on a consistent scale.

### Use constrained layout

```python
fig, axes = plt.subplots(
    2, 2,
    figsize=(10, 8),
    layout="constrained"
)
```

This helps manage spacing between subplots, labels, and titles.

---

## 11. Working with NumPy and Pandas

### Plot NumPy arrays

```python
x = np.arange(1, 11)
y = x ** 2

fig, ax = plt.subplots()

ax.plot(x, y)
ax.set_xlabel("x")
ax.set_ylabel("x²")

plt.show()
```

### Plot a Pandas DataFrame

```python
df = pd.DataFrame({
    "Month": ["Jan", "Feb", "Mar", "Apr"],
    "Sales": [100, 120, 115, 150],
    "Expenses": [70, 80, 75, 95]
})

fig, ax = plt.subplots()

ax.plot(df["Month"], df["Sales"], label="Sales")
ax.plot(df["Month"], df["Expenses"], label="Expenses")

ax.set_xlabel("Month")
ax.set_ylabel("Amount")
ax.legend()

plt.show()
```

### Pandas built-in plotting

Pandas plotting uses Matplotlib by default.

```python
df.plot(
    x="Month",
    y=["Sales", "Expenses"],
    kind="line",
    marker="o"
)

plt.show()
```

### Plot a correlation matrix

```python
df_numeric = df.select_dtypes(include="number")
correlation = df_numeric.corr()

fig, ax = plt.subplots()

image = ax.imshow(
    correlation,
    cmap="coolwarm",
    vmin=-1,
    vmax=1
)

ax.set_xticks(range(len(correlation.columns)), correlation.columns)
ax.set_yticks(range(len(correlation.index)), correlation.index)

fig.colorbar(image, ax=ax, label="Correlation")

plt.show()
```

For a large matrix, rotate x-axis labels and adjust figure size for readability.

---

## 12. Statistical Visualizations

### Compare distributions

```python
group_a = np.random.normal(50, 10, 200)
group_b = np.random.normal(60, 12, 200)

fig, ax = plt.subplots()

ax.hist(group_a, bins=20, alpha=0.5, label="Group A")
ax.hist(group_b, bins=20, alpha=0.5, label="Group B")

ax.set_xlabel("Value")
ax.set_ylabel("Frequency")
ax.legend()

plt.show()
```

### Compare medians and spread

```python
fig, ax = plt.subplots()

ax.boxplot(
    [group_a, group_b],
    tick_labels=["Group A", "Group B"]
)

ax.set_ylabel("Value")
plt.show()
```

Note: `tick_labels` is supported by modern Matplotlib releases. Older versions may use `labels`.

### Visualize a trend with a reference line

```python
x = np.arange(1, 11)
y = np.array([10, 12, 15, 14, 18, 20, 19, 23, 25, 28])

fig, ax = plt.subplots()

ax.plot(x, y, marker="o", label="Observed")
ax.axhline(y.mean(), linestyle="--", label="Mean")

ax.legend()
plt.show()
```

**Interpretation tip:** A chart describes patterns in the observed data; it does not by itself establish causation or statistical significance.

---

## 13. Customizing Styles

### Inspect available styles

```python
print(plt.style.available)
```

### Apply a built-in style

```python
plt.style.use("ggplot")
```

### Use a temporary style

```python
with plt.style.context("ggplot"):
    fig, ax = plt.subplots()
    ax.plot([1, 2, 3], [10, 20, 15])
    plt.show()
```

### Set global defaults

```python
plt.rcParams["figure.figsize"] = (8, 5)
plt.rcParams["font.size"] = 11
plt.rcParams["axes.grid"] = True
```

Global `rcParams` settings affect subsequent figures. Use a temporary style context when you want to limit styling changes to a specific section of code.

---

## 14. Saving and Exporting Figures

### Save as PNG

```python
fig, ax = plt.subplots()

ax.plot([1, 2, 3], [10, 20, 15])
ax.set_title("Sample Chart")

fig.savefig("chart.png", dpi=300, bbox_inches="tight")
```

### Save as PDF

```python
fig.savefig("chart.pdf", bbox_inches="tight")
```

### Save as SVG

```python
fig.savefig("chart.svg", bbox_inches="tight")
```

### Transparent background

```python
fig.savefig(
    "chart.png",
    dpi=300,
    transparent=True,
    bbox_inches="tight"
)
```

### Important parameters

| Parameter | Purpose |
|---|---|
| `dpi=300` | Higher raster output resolution |
| `bbox_inches="tight"` | Reduces unnecessary surrounding whitespace |
| `transparent=True` | Makes the saved background transparent |
| `format="pdf"` | Explicitly sets the output format |

Call `fig.savefig()` before closing the figure. For publication or print, PDF and SVG preserve vector elements where supported, while PNG is useful for raster images.

---

## 15. Useful Functions Reference

### Figure and axes

| Function | Purpose |
|---|---|
| `plt.subplots()` | Create a figure and axes |
| `plt.figure()` | Create a figure |
| `fig.add_subplot()` | Add an axes to a figure |
| `fig.suptitle()` | Add a figure-level title |
| `plt.show()` | Display figures |
| `plt.close()` | Close a figure |
| `fig.savefig()` | Save a figure |

### Plotting functions

| Function | Purpose |
|---|---|
| `ax.plot()` | Line plot |
| `ax.scatter()` | Scatter plot |
| `ax.bar()` | Vertical bar chart |
| `ax.barh()` | Horizontal bar chart |
| `ax.hist()` | Histogram |
| `ax.boxplot()` | Box plot |
| `ax.pie()` | Pie chart |
| `ax.fill_between()` | Filled area between curves |
| `ax.step()` | Step plot |
| `ax.errorbar()` | Plot with error bars |
| `ax.imshow()` | Display a 2D array as an image |
| `ax.hexbin()` | Hexagonal binning for dense 2D data |

### Customization functions

| Function | Purpose |
|---|---|
| `ax.set_title()` | Set chart title |
| `ax.set_xlabel()` | Set x-axis label |
| `ax.set_ylabel()` | Set y-axis label |
| `ax.legend()` | Display legend |
| `ax.grid()` | Show or configure grid |
| `ax.set_xlim()` | Set x-axis limits |
| `ax.set_ylim()` | Set y-axis limits |
| `ax.set_xticks()` | Set x-axis tick positions and optional labels |
| `ax.set_yticks()` | Set y-axis tick positions and optional labels |
| `ax.tick_params()` | Customize tick appearance |
| `ax.text()` | Add text |
| `ax.annotate()` | Annotate a point |
| `ax.axhline()` | Add a horizontal reference line |
| `ax.axvline()` | Add a vertical reference line |
| `ax.set_xscale()` | Set x-axis scale |
| `ax.set_yscale()` | Set y-axis scale |

---

## 16. Common Plotting Patterns

### Pattern 1: Reusable plotting function

```python
import matplotlib.pyplot as plt

def plot_series(x, y, title, xlabel, ylabel):
    fig, ax = plt.subplots(figsize=(8, 5))

    ax.plot(x, y, marker="o")
    ax.set_title(title)
    ax.set_xlabel(xlabel)
    ax.set_ylabel(ylabel)
    ax.grid(True, alpha=0.3)

    return fig, ax


fig, ax = plot_series(
    [1, 2, 3, 4],
    [10, 15, 13, 20],
    "Monthly Sales",
    "Month",
    "Sales"
)

plt.show()
```

Returning the figure and axes lets callers further customize the chart or save it.

### Pattern 2: Handle missing data

```python
df = pd.DataFrame({
    "x": [1, 2, 3, 4, 5],
    "y": [10, None, 15, 20, 18]
})

fig, ax = plt.subplots()

ax.plot(df["x"], df["y"], marker="o")
plt.show()
```

Missing y-values are normally represented as gaps in a line. Check missing values and decide whether to retain, remove, or impute them based on the analysis.

### Pattern 3: Plot dates

```python
dates = pd.date_range("2026-01-01", periods=6, freq="MS")
sales = [100, 120, 115, 140, 135, 160]

fig, ax = plt.subplots()

ax.plot(dates, sales, marker="o")
ax.set_xlabel("Date")
ax.set_ylabel("Sales")
fig.autofmt_xdate()

plt.show()
```

### Pattern 4: Create a confusion matrix visualization

```python
from sklearn.metrics import confusion_matrix

y_true = [0, 0, 1, 1, 1, 0]
y_pred = [0, 1, 1, 1, 0, 0]

cm = confusion_matrix(y_true, y_pred)

fig, ax = plt.subplots()

image = ax.imshow(cm, cmap="Blues")
fig.colorbar(image, ax=ax)

ax.set_xlabel("Predicted label")
ax.set_ylabel("True label")
ax.set_title("Confusion Matrix")

for i in range(cm.shape[0]):
    for j in range(cm.shape[1]):
        ax.text(j, i, str(cm[i, j]), ha="center", va="center")

plt.show()
```

For machine learning projects, label the class names and confirm the ordering of the labels when interpreting the matrix.

---

## 17. Best Practices

1. **Choose the right chart.** Use lines for trends, bars for category comparisons, scatter plots for relationships, and histograms for distributions.
2. **Label clearly.** Include meaningful titles, axis names, and units.
3. **Use the object-oriented interface.** It improves control over complex charts and reusable functions.
4. **Avoid misleading scales.** Clearly communicate axis ranges and logarithmic scales.
5. **Limit visual clutter.** Avoid excessive gridlines, decorations, colors, and labels.
6. **Use consistent scales for comparisons.** Different axis limits can make similar datasets appear misleadingly different.
7. **Make charts accessible.** Use readable fonts, distinguishable colors, and adequate contrast.
8. **Handle missing and invalid values.** Inspect data before plotting.
9. **Choose appropriate output formats.** Use PNG for raster images and SVG or PDF when vector output is useful.
10. **Close figures in batch scripts.** Use `plt.close(fig)` after saving figures to release resources.
11. **Avoid unnecessary global state.** Prefer explicit `fig` and `ax` references in reusable code.
12. **Explain uncertainty.** Distinguish standard deviation, standard error, confidence intervals, and other uncertainty measures when plotting them.

---

## 18. Common Errors and Solutions

| Problem | Likely cause | Solution |
|---|---|---|
| Plot window does not appear | Backend or environment issue | Check the environment and display backend; use a supported GUI backend or notebook |
| Lines do not connect as expected | x-values are unordered | Sort observations by x when a continuous ordered line is intended |
| `ValueError` about dimensions | x and y have incompatible lengths | Check shapes and lengths before plotting |
| Too many x-axis labels | Many categorical values | Rotate, reduce, or format ticks |
| Text is cut off | Figure layout is too tight | Use `layout="constrained"` or adjust figure size |
| Saved image is blank | Figure saved too late or wrong figure selected | Save the intended `fig` before closing it |
| Log plot fails or omits values | Non-positive values | Inspect the data and choose a suitable scale |
| Categories appear in an unexpected order | Automatic categorical ordering | Explicitly specify the intended category order |
| Figures consume too much memory | Many figures remain open | Close figures after use in batch workflows |
| `NameError: plt is not defined` | Missing import | Add `import matplotlib.pyplot as plt` |

### Troubleshooting example

```python
fig, ax = plt.subplots(figsize=(8, 5), layout="constrained")

ax.plot(x, y)
ax.set_title("Clear Chart")
ax.set_xlabel("X")
ax.set_ylabel("Y")

fig.savefig("output.png", dpi=300)
plt.show()
```

---

## 19. Practice Exercises

Use a notebook to solve the following exercises. Try each exercise before consulting the official documentation.

### Beginner

- [ ] Create a line chart showing daily temperatures.
- [ ] Create a bar chart comparing five products.
- [ ] Plot a histogram of 1,000 randomly generated values.
- [ ] Add titles, labels, a legend, and gridlines.
- [ ] Change the marker, line style, and figure size.

### Intermediate

- [ ] Create four charts using a 2 × 2 subplot layout.
- [ ] Compare two groups using overlapping histograms.
- [ ] Create a scatter plot with marker sizes or colors representing a third variable.
- [ ] Plot a time-series dataset using Pandas.
- [ ] Save a chart as PNG, PDF, and SVG.
- [ ] Create a heatmap from a correlation matrix.

### Advanced

- [ ] Create a reusable plotting function with configurable labels and styles.
- [ ] Visualize a machine learning confusion matrix with class names.
- [ ] Build a dashboard showing sales, expenses, and profit.
- [ ] Create a chart containing annotations and target reference lines.
- [ ] Visualize model prediction errors and residual distributions.
- [ ] Design an accessible chart suitable for a technical report.

### Mini-project: Sales Analysis Dashboard

Create a small dataset with columns for date, product, sales, and expenses.

Build a dashboard containing:

1. Monthly sales trend.
2. Sales by product.
3. Distribution of transaction values.
4. Sales versus expenses.
5. A summary of the main findings.

**Expected outcome:** A multi-chart visualization that demonstrates data preparation, chart selection, formatting, and interpretation.

---

## 20. Quick Revision

| Requirement | Recommended method |
|---|---|
| Create a chart | `fig, ax = plt.subplots()` |
| Draw a line | `ax.plot(x, y)` |
| Compare categories | `ax.bar(categories, values)` |
| Plot relationships | `ax.scatter(x, y)` |
| Examine distributions | `ax.hist(data, bins=20)` |
| Summarize spread | `ax.boxplot(data)` |
| Add a title | `ax.set_title("Title")` |
| Label axes | `ax.set_xlabel()` and `ax.set_ylabel()` |
| Show legend | `ax.legend()` |
| Add grid | `ax.grid(True)` |
| Create subplots | `plt.subplots(rows, cols)` |
| Set axis limits | `ax.set_xlim()` and `ax.set_ylim()` |
| Change scale | `ax.set_xscale()` and `ax.set_yscale()` |
| Add reference line | `ax.axhline()` or `ax.axvline()` |
| Save a figure | `fig.savefig("chart.png", dpi=300)` |
| Close a figure | `plt.close(fig)` |

### Standard plotting template

```python
import matplotlib.pyplot as plt

x = [1, 2, 3, 4, 5]
y = [10, 20, 15, 30, 25]

fig, ax = plt.subplots(
    figsize=(8, 5),
    layout="constrained"
)

ax.plot(x, y, marker="o", label="Series A")

ax.set_title("Chart Title")
ax.set_xlabel("X Label")
ax.set_ylabel("Y Label")

ax.grid(True, alpha=0.3)
ax.legend()

fig.savefig("chart.png", dpi=300)
plt.show()
```

---

## 21. Official Resources

- [Matplotlib Official Documentation](https://matplotlib.org/stable/)
- [Matplotlib Quick Start Guide](https://matplotlib.org/stable/users/explain/quick_start.html)
- [Matplotlib Plot Types](https://matplotlib.org/stable/plot_types/index.html)
- [Matplotlib Tutorials](https://matplotlib.org/stable/tutorials/index.html)
- [Matplotlib API Reference](https://matplotlib.org/stable/api/index.html)
- [Official Matplotlib Cheat Sheets](https://github.com/matplotlib/cheatsheets)
- [NumPy Documentation](https://numpy.org/doc/)
- [Pandas Visualization Guide](https://pandas.pydata.org/docs/user_guide/visualization.html)

---

**Next topics:** [Seaborn](https://seaborn.pydata.org/), [Plotly](https://plotly.com/python/), and exploratory data analysis (EDA).

**Maintained by:** [CubicNova](https://github.com/cubicnova/Data-Science-Roadmap)



