# Seaborn

**Module:** `06-Data-Visualization/03-Seaborn`  
**Level:** Beginner to intermediate  
**Focus:** Statistical data visualization with Seaborn and Pandas.

## Overview

Seaborn is a Python library for statistical data visualization. It works closely with Pandas DataFrames and Matplotlib, offering high-level functions for exploring relationships, distributions, categories, and multivariate data.

Seaborn provides helpful defaults, but a good chart still depends on sound data preparation, appropriate chart selection, and careful interpretation.

## Learning objectives

By the end of this module, you should be able to:

- Install and import Seaborn.
- Use DataFrames as the source for plots.
- Create relational plots with `scatterplot()`, `lineplot()`, and `relplot()`.
- Visualize distributions with histograms, KDE plots, and ECDF plots.
- Compare groups using strip, swarm, box, violin, bar, and count plots.
- Use `hue`, `size`, and `style` to represent additional variables.
- Build faceted plots with `relplot()`, `displot()`, and `catplot()`.
- Explore correlations with heatmaps.
- Create regression visualizations and interpret them carefully.
- Customize themes, palettes, labels, and figure layouts.

## Prerequisites

- Basic Python and Pandas.
- Basic chart interpretation.
- Familiarity with Matplotlib fundamentals from `01-Visualization-Fundamentals` and `02-Matplotlib`.

## Installation

```bash
python -m pip install seaborn pandas matplotlib numpy jupyter
```

Launch Jupyter Notebook:

```bash
jupyter notebook
```

Open `examples.ipynb` and run the cells from top to bottom. The notebook uses locally defined sample data so the core examples do not require a network connection.

## 1. Import and prepare data

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

df = pd.DataFrame({
    "study_hours": [1, 2, 3, 4, 5, 6],
    "score": [52, 57, 65, 70, 78, 85],
    "group": ["A", "A", "B", "B", "A", "B"]
})
```

Seaborn functions often accept a DataFrame through `data=` and refer to columns by name, making plotting code concise and readable.

## 2. Relational plots

Relational plots help explore how variables change together.

### Scatter plot

```python
sns.scatterplot(data=df, x="study_hours", y="score", hue="group")
plt.title("Study Hours and Scores")
plt.show()
```

A scatter plot shows individual observations. `hue`, `style`, and `size` can encode additional variables. Use these mappings thoughtfully to avoid clutter.

### Line plot

```python
monthly = pd.DataFrame({
    "month": ["Jan", "Feb", "Mar", "Apr", "May"],
    "revenue": [120, 135, 128, 160, 172]
})
sns.lineplot(data=monthly, x="month", y="revenue", marker="o")
plt.title("Monthly Revenue")
plt.show()
```

Line plots are suitable for ordered values, especially time. When there are repeated observations at each x value, Seaborn may aggregate them and display uncertainty by default. Set `errorbar=None` when you intentionally do not want an uncertainty band.

### Figure-level relational plots

```python
sns.relplot(data=df, x="study_hours", y="score", hue="group", kind="scatter")
```

`relplot()` is a figure-level interface that can create facets using `col=` and `row=`. `scatterplot()` and `lineplot()` are axes-level functions.

## 3. Distribution plots

### Histogram

```python
scores = [52, 57, 60, 63, 65, 68, 70, 72, 78, 81, 85, 91]
sns.histplot(scores, bins=6, kde=True)
plt.title("Score Distribution")
plt.show()
```

Histograms show counts or proportions in bins. The choice of bin width influences the apparent distribution.

### Kernel density estimate (KDE)

```python
sns.kdeplot(data=df, x="score", fill=True)
plt.title("Estimated Score Density")
plt.show()
```

KDE estimates a smooth distribution. Its shape depends on the bandwidth and can be misleading with small samples or discrete data. Consider displaying the observations or a histogram as context.

### ECDF

```python
sns.ecdfplot(data=df, x="score")
plt.title("Empirical Cumulative Distribution")
plt.show()
```

An ECDF shows the fraction of observations at or below each value and avoids choosing histogram bins.

### Figure-level distribution plot

```python
sns.displot(data=df, x="score", hue="group", kind="hist", bins=6)
```

Use `displot()` when you want figure-level behavior or facets such as `col=` and `row=`.

## 4. Categorical plots

Categorical plots compare measurements across groups.

| Function | Main purpose |
|---|---|
| `stripplot()` | Show individual observations with jitter |
| `swarmplot()` | Show individual points with overlap reduction |
| `boxplot()` | Compare quartiles, medians, and potential outliers |
| `violinplot()` | Compare estimated distribution shapes |
| `barplot()` | Compare an estimate, usually a mean, with uncertainty |
| `countplot()` | Count observations in each category |
| `pointplot()` | Compare estimates using points and intervals |

Examples:

```python
sns.boxplot(data=df, x="group", y="score")
plt.show()

sns.violinplot(data=df, x="group", y="score", inner="quart")
plt.show()

sns.countplot(data=df, x="group")
plt.show()
```

Be aware that `barplot()` generally estimates a summary statistic and may show uncertainty; it is not simply a count of rows. Use `countplot()` to display category counts.

`catplot()` provides a figure-level interface for categorical chart types and supports faceting:

```python
sns.catplot(data=df, x="group", y="score", kind="box")
```

## 5. Add information with `hue`

The `hue` parameter maps a variable to color:

```python
sns.scatterplot(data=df, x="study_hours", y="score", hue="group", style="group")
```

Color can distinguish categories or represent a numeric gradient. Keep the legend clear and use markers or line styles as additional cues when accessibility matters.

## 6. Faceting and small multiples

Faceting creates a set of comparable charts for different subsets of data.

```python
sns.relplot(
    data=df,
    x="study_hours",
    y="score",
    col="group",
    kind="scatter"
)
```

Small multiples can reveal group differences without placing every series on a single crowded chart. Keep axes comparable when direct comparison is intended.

## 7. Heatmaps and correlation

A heatmap encodes values using color. A common use is displaying a correlation matrix:

```python
numeric = df[["study_hours", "score"]]
correlation = numeric.corr()

sns.heatmap(correlation, annot=True, vmin=-1, vmax=1, cmap="vlag")
plt.title("Correlation Matrix")
plt.show()
```

Correlation measures association, not causation. Correlations may also be affected by outliers, non-linear relationships, and subgroup structure.

## 8. Regression plots

```python
sns.regplot(data=df, x="study_hours", y="score")
plt.title("Study Hours and Score: Linear Fit")
plt.show()
```

Regression plots add a fitted line to help inspect a possible trend. They are exploratory visuals, not a replacement for model diagnostics or statistical analysis. Avoid interpreting a fitted line as proof of causation.

## 9. Themes and palettes

```python
sns.set_theme(style="whitegrid", context="notebook")
sns.set_palette("deep")
```

Common styles include `darkgrid`, `whitegrid`, `dark`, `white`, and `ticks`. Palette families include qualitative palettes for categories, sequential palettes for ordered numeric values, and diverging palettes for values centered around a meaningful midpoint.

Choose palettes for meaning and readability, not decoration. Do not rely on color alone to communicate critical distinctions.

## 10. Customize titles and labels

```python
ax = sns.scatterplot(data=df, x="study_hours", y="score", hue="group")
ax.set(
    title="Study Hours vs Assessment Score",
    xlabel="Study hours",
    ylabel="Assessment score"
)
plt.tight_layout()
plt.show()
```

Axes-level functions generally return an Axes object. Figure-level functions such as `relplot()`, `displot()`, and `catplot()` return a grid object; customize its axes or figure accordingly.

## 11. Common mistakes

- Using a count plot when the question asks for a numerical summary, or using a bar plot when it asks for category counts.
- Treating a KDE curve as exact, especially with very small samples.
- Adding too many `hue`, `style`, and `size` mappings.
- Comparing facets with inconsistent axes when direct comparison is needed.
- Forgetting that line plots can aggregate repeated observations.
- Interpreting correlation or regression as causal proof.
- Using a palette with poor contrast or relying on color alone.
- Ignoring missing data and outliers before plotting.

## Mini-project: Student performance exploration

Using the sample dataset in `examples.ipynb`:

1. Compare scores across groups with a box plot and violin plot.
2. Explore the relationship between study hours and score with a scatter plot.
3. Add `hue` to distinguish groups.
4. Plot the score distribution with a histogram and KDE.
5. Create a correlation heatmap for numeric variables.
6. Write three observations and two limitations.

## Files in this module

- [`README.md`](README.md) — Seaborn concepts and chart guide.
- [`examples.ipynb`](examples.ipynb) — runnable examples.
- [`practice.md`](practice.md) — progressive exercises.
- [`solutions.md`](solutions.md) — reference solutions.

## Further reading

- [Seaborn user guide and tutorial](https://seaborn.pydata.org/tutorial.html)
- [Relational plots](https://seaborn.pydata.org/tutorial/relational.html)
- [Categorical plots](https://seaborn.pydata.org/tutorial/categorical.html)
- [Seaborn API reference](https://seaborn.pydata.org/api.html)

## Completion checklist

- [ ] I can create relational and distribution plots.
- [ ] I can compare groups with categorical plots.
- [ ] I can use `hue` and faceting to explore additional variables.
- [ ] I can build a correlation heatmap and a regression plot.
- [ ] I can select suitable themes and palettes.
- [ ] I completed the practice exercises and mini-project.
