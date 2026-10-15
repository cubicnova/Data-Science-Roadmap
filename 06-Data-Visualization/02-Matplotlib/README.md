# Matplotlib

**Module:** `06-Data-Visualization/02-Matplotlib`  
**Level:** Beginner to intermediate  
**Focus:** Create, customize, arrange, and export visualizations with Matplotlib.

## Overview

Matplotlib is a Python visualization library used to create static, animated, and interactive visualizations. This module focuses on practical plotting with `matplotlib.pyplot` and the object-oriented `Figure` / `Axes` interface.

The object-oriented pattern is recommended for reusable charts:

```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots()
ax.plot([1, 2, 3], [2, 4, 3])
ax.set(title="Example", xlabel="X", ylabel="Y")
fig.tight_layout()
plt.show()
```

## Learning objectives

By the end of this module, you should be able to:

- Install and import Matplotlib.
- Understand the relationship between a Figure and its Axes.
- Create line, bar, horizontal bar, scatter, histogram, pie, and box plots.
- Customize titles, labels, ticks, legends, grid lines, markers, and line styles.
- Arrange multiple plots with subplots.
- Add text and annotations.
- Control axis limits and scales.
- Save figures to image files.
- Apply good practices for clear, accessible, and honest charts.

## Prerequisites

- Basic Python syntax, lists, and functions.
- Basic data handling with lists, NumPy arrays, or Pandas DataFrames.
- Familiarity with the visualization concepts in `01-Visualization-Fundamentals`.

## Installation

```bash
python -m pip install matplotlib numpy pandas jupyter
```

Start Jupyter Notebook:

```bash
jupyter notebook
```

Open `examples.ipynb` and execute the notebook from top to bottom. All examples use small, locally defined datasets.

## 1. Matplotlib components

- **Figure:** the complete canvas or window containing the chart.
- **Axes:** the plotting area where data is drawn; it includes the x-axis, y-axis, labels, title, and plotted artists.
- **Axis:** controls a coordinate dimension, including ticks and tick labels.
- **Artist:** a drawable object such as a line, marker, text label, patch, or legend.

A Figure can contain one or multiple Axes.

## 2. Two common interfaces

### Pyplot interface

Convenient for quick experiments:

```python
import matplotlib.pyplot as plt

plt.plot([1, 2, 3], [2, 4, 3])
plt.title("Quick line plot")
plt.xlabel("X")
plt.ylabel("Y")
plt.show()
```

### Object-oriented interface

Recommended for more control, reusable functions, and multiple charts:

```python
fig, ax = plt.subplots(figsize=(7, 4))
ax.plot([1, 2, 3], [2, 4, 3], marker="o", label="Series A")
ax.set(title="Object-oriented plot", xlabel="X", ylabel="Y")
ax.legend()
fig.tight_layout()
plt.show()
```

Use `ax` methods to configure a specific plot and `fig` methods to configure the whole figure.

## 3. Common chart types

### Line plot

Best for ordered values and trends, often across time.

```python
months = ["Jan", "Feb", "Mar", "Apr"]
sales = [120, 135, 128, 160]

fig, ax = plt.subplots()
ax.plot(months, sales, marker="o", linestyle="-", label="Sales")
ax.set(title="Monthly Sales", xlabel="Month", ylabel="Sales")
ax.legend()
plt.show()
```

### Bar and horizontal bar plots

Use bars to compare categories.

```python
fig, ax = plt.subplots()
ax.bar(["A", "B", "C"], [24, 35, 19])
ax.set(title="Values by Category", xlabel="Category", ylabel="Value")
plt.show()
```

For long category labels, `ax.barh()` can be easier to read.

### Scatter plot

Use to examine the relationship between two numerical variables.

```python
fig, ax = plt.subplots()
ax.scatter([1, 2, 3, 4], [2, 3, 5, 4], alpha=0.75)
ax.set(title="X and Y Relationship", xlabel="X", ylabel="Y")
plt.show()
```

An association in a scatter plot does not prove causation.

### Histogram

Use to examine the distribution of a numerical variable. Bin count affects the apparent shape.

```python
fig, ax = plt.subplots()
ax.hist([12, 15, 15, 18, 19, 21, 25, 28, 30], bins=5, edgecolor="black")
ax.set(title="Value Distribution", xlabel="Value", ylabel="Frequency")
plt.show()
```

### Box plot

Summarizes a distribution with median, quartiles, and whiskers. The default whisker rule should be understood before interpreting points beyond the whiskers.

```python
fig, ax = plt.subplots()
ax.boxplot([[10, 12, 13, 15, 18], [8, 11, 14, 17, 22]],
           tick_labels=["Group A", "Group B"])
ax.set(title="Group Comparison", ylabel="Value")
plt.show()
```

### Pie chart

Use only for a small number of mutually exclusive parts of a whole when the whole is meaningful. For close comparisons, a bar chart is usually clearer.

```python
fig, ax = plt.subplots()
ax.pie([45, 30, 25], labels=["A", "B", "C"], autopct="%1.1f%%")
ax.set_title("Share by Category")
plt.show()
```

## 4. Customization essentials

```python
fig, ax = plt.subplots(figsize=(8, 4))
ax.plot([1, 2, 3], [3, 5, 4], marker="s", linestyle="--",
        linewidth=2, label="Example")
ax.set_title("Customized Chart", fontsize=14)
ax.set_xlabel("Time")
ax.set_ylabel("Value")
ax.set_xticks([1, 2, 3])
ax.grid(axis="y", alpha=0.25)
ax.legend()
fig.tight_layout()
plt.show()
```

Useful options include:

- `figsize=(width, height)` to control figure size.
- `marker`, `linestyle`, and `linewidth` to distinguish line series.
- `alpha` to control transparency.
- `set_xlim()` and `set_ylim()` to set visible limits.
- `set_xticks()` and `set_yticks()` to control tick locations.
- `legend()` to identify multiple series.
- `annotate()` to point out a notable value or event.
- `tight_layout()` or `layout="constrained"` to improve spacing.

## 5. Multiple plots with subplots

```python
fig, axes = plt.subplots(1, 2, figsize=(10, 4))

axes[0].bar(["A", "B", "C"], [4, 7, 5])
axes[0].set_title("Category Comparison")

axes[1].plot([1, 2, 3, 4], [3, 5, 4, 8], marker="o")
axes[1].set_title("Trend")

fig.tight_layout()
plt.show()
```

`plt.subplots(rows, columns)` returns a Figure and one or more Axes. For grids, use `fig, axes = plt.subplots(2, 2)` and access individual axes with `axes[row, column]`.

## 6. Annotations and reference lines

```python
fig, ax = plt.subplots()
x = [1, 2, 3, 4]
y = [10, 14, 12, 20]
ax.plot(x, y, marker="o")
ax.axhline(15, linestyle="--", label="Target")
ax.annotate("Peak", xy=(4, 20), xytext=(3, 21),
            arrowprops={"arrowstyle": "->"})
ax.set(title="Performance vs Target", xlabel="Period", ylabel="Value")
ax.legend()
plt.show()
```

Annotations should clarify an important feature rather than overload the chart.

## 7. Axis limits and scales

```python
ax.set_xlim(0, 10)
ax.set_ylim(0, 100)
```

Logarithmic scales can be useful when positive values span several orders of magnitude:

```python
ax.set_yscale("log")
```

Always label the scale clearly and consider whether the audience will interpret it correctly. Log scales cannot display zero or negative values in the ordinary way.

## 8. Saving charts

Save a figure before or after displaying it:

```python
fig, ax = plt.subplots(figsize=(7, 4))
ax.plot([1, 2, 3], [2, 4, 3])
ax.set(title="Export Example", xlabel="X", ylabel="Y")
fig.tight_layout()
fig.savefig("export-example.png", dpi=150, bbox_inches="tight")
```

Common formats include PNG, JPEG, SVG, and PDF. Raster formats such as PNG are useful for screens; SVG and PDF are useful for scalable output. The output path is relative to the notebook's current working directory unless an absolute path is supplied.

## 9. Good practices

- Prefer the object-oriented interface for multi-chart work.
- Give every chart a meaningful title and axes labels with units.
- Use legends only when needed and ensure labels identify the correct series.
- Avoid unnecessary 3D effects and decorative clutter.
- Keep comparable charts on comparable scales.
- Do not use color as the only way to distinguish series.
- Use sensible bin widths for histograms.
- Avoid truncated bar-chart axes when bar length encodes magnitude.
- Close figures in long-running scripts when they are no longer needed: `plt.close(fig)`.
- Make charts large enough to read and check that labels are not clipped.

## Mini-project: Sales performance dashboard

Use the dataset in `examples.ipynb` to create a two-by-two figure containing:

1. Monthly revenue line chart.
2. Revenue by product bar chart.
3. Distribution of order values as a histogram.
4. Advertising spend versus revenue scatter plot.

Add an overall title, clear labels, and a brief written interpretation. Save the figure as `sales-dashboard.png`.

## Files in this module

- [`README.md`](README.md) — Matplotlib concepts and reference examples.
- [`examples.ipynb`](examples.ipynb) — runnable tutorial notebook.
- [`practice.md`](practice.md) — progressive exercises.
- [`solutions.md`](solutions.md) — reference solutions.

## Further reading

- [Matplotlib Pyplot tutorial](https://matplotlib.org/stable/tutorials/pyplot.html)
- [Introduction to Axes and subplots](https://matplotlib.org/stable/users/explain/axes/axes_intro.html)
- [Matplotlib examples gallery](https://matplotlib.org/stable/gallery/index.html)

## Completion checklist

- [ ] I understand Figures, Axes, and the pyplot interface.
- [ ] I can create and customize common plot types.
- [ ] I can create multiple subplots and annotate a chart.
- [ ] I can set axis limits and scales appropriately.
- [ ] I can save a chart to a file.
- [ ] I completed the practice exercises and mini-project.
