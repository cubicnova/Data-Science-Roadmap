# Chart Selection

**Module:** `06-Data-Visualization/05-Chart-Selection`  
**Level:** Beginner to intermediate  
**Focus:** Choose an effective chart based on the analytical question, data types, audience, and context.

## Overview

Chart selection is the process of choosing a visual form that helps an audience answer a specific question accurately. A chart can be technically correct yet misleading or confusing if it does not fit the question.

Start with the question, then identify the data types and the comparison you want readers to make. The recommendations in this module are starting points rather than rigid rules: context, sample size, data quality, and audience all matter.

## Learning objectives

By the end of this module, you should be able to:

- Translate an analytical question into a chart-selection task.
- Identify common chart goals: comparison, trend, distribution, relationship, composition, ranking, and geographic patterns.
- Match common data types and questions to appropriate chart types.
- Explain when to use bar, line, scatter, histogram, box, violin, heatmap, and composition charts.
- Recognize when a chart is inappropriate or likely to mislead.
- Choose between counts, rates, totals, and summary statistics.
- Consider audience, accessibility, uncertainty, and sample size.
- Build a simple chart-selection decision workflow.
- Implement selected charts with Matplotlib and Seaborn.

## Prerequisites

- Basic Python and Pandas.
- Familiarity with visualization fundamentals, Matplotlib, and Seaborn.
- Basic understanding of categorical and numerical variables.

## Setup

```bash
python -m pip install pandas numpy matplotlib seaborn jupyter
```

Open `examples.ipynb` in Jupyter Notebook or JupyterLab and run the cells from top to bottom. All sample data is generated in the notebook.

## 1. Start with the question

Before choosing a chart, answer these questions:

1. **What should the reader learn?** For example, compare categories, inspect a trend, or understand a distribution.
2. **What are the variables?** Are they categorical, numerical, temporal, ordinal, or geographic?
3. **What comparison matters?** Absolute values, proportions, change, spread, or association?
4. **How much data is there?** Many categories or overlapping observations may need aggregation or a different view.
5. **Who is the audience?** Choose a level of complexity and terminology they can understand.
6. **What context is essential?** Include units, time range, denominator, sample size, source, and uncertainty when relevant.

A chart should answer a question—not merely decorate a report.

## 2. Chart-selection quick reference

| Goal / question | Good starting choice | Notes |
|---|---|---|
| Compare values across categories | Bar chart / dot plot | Sort categories when useful; bars usually start at zero |
| Show ranking | Sorted horizontal bar chart | Useful for long category labels |
| Show change over time | Line chart | Time must be ordered; avoid connecting unrelated categories |
| Compare two numeric variables | Scatter plot | Association does not imply causation |
| Examine one numeric distribution | Histogram / ECDF / box plot | Histogram depends on bin width; ECDF avoids bins |
| Compare distributions across groups | Box plot / violin plot / strip plot | Show individual points when sample size allows |
| Show frequency of categories | Count plot / bar chart of counts | Check whether counts should be normalized to rates |
| Show parts of a whole | Stacked bar; sometimes pie chart | Parts should be mutually exclusive and share a meaningful whole |
| Compare composition across groups | 100% stacked bar | Harder to compare segments not aligned to a common baseline |
| Show correlation across numeric features | Correlation heatmap | Correlation is not causation; inspect nonlinear patterns too |
| Show relationship with many overlapping points | Transparency, hexbin, or density plot | Avoid hiding density through overplotting |
| Compare values across two categorical dimensions | Grouped bar chart or heatmap | Choose based on number of groups and precision needed |
| Show geographic variation | Map / choropleth | Use rates or normalized measures when populations differ |
| Show uncertainty around an estimate | Error bars, confidence interval plot, or interval plot | State what the interval represents |
| Show a process or sequence of stages | Flow diagram or funnel chart | Ensure stage definitions and denominators are clear |

## 3. Common chart types

### Bar chart

Use for comparing categories or ranking items. A bar's length encodes magnitude, so a zero baseline is generally important. Horizontal bars are useful for long labels.

```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots()
ax.bar(["North", "South", "West"], [42, 35, 51])
ax.set(title="Orders by Region", xlabel="Region", ylabel="Orders")
plt.show()
```

### Line chart

Use for ordered observations, especially time series. Sort timestamps and avoid implying continuity between unrelated categories.

```python
months = ["Jan", "Feb", "Mar", "Apr"]
revenue = [120, 135, 128, 160]

fig, ax = plt.subplots()
ax.plot(months, revenue, marker="o")
ax.set(title="Monthly Revenue", xlabel="Month", ylabel="Revenue")
plt.show()
```

### Scatter plot

Use to explore a relationship between two numerical variables. Look for direction, form, clusters, and unusual observations. Consider transparency or aggregation when points overlap.

```python
fig, ax = plt.subplots()
ax.scatter([1, 2, 3, 4, 5], [52, 57, 65, 70, 82], alpha=0.7)
ax.set(title="Study Hours and Score", xlabel="Study hours", ylabel="Score")
plt.show()
```

### Histogram and ECDF

Use a histogram to examine distribution shape and frequency across bins. Use an empirical cumulative distribution function (ECDF) to see the proportion of observations at or below each value without selecting bins.

### Box plot and violin plot

Use these to compare distributions across groups. Box plots summarize median, quartiles, and whiskers. Violin plots show a smoothed density estimate, which can be unreliable with very small samples.

### Stacked bars and pie charts

Use composition charts only when the parts add up to a meaningful whole. A stacked bar works well when comparing totals or a small number of segments. A 100% stacked bar emphasizes proportions. Pie charts are best limited to a few clearly distinct parts; bar charts are usually better for precise comparisons.

### Heatmap

Use a heatmap to show values in a matrix, such as correlations or a category-by-time table. Choose a color scale that matches the data: sequential for low-to-high values and diverging for values around a meaningful midpoint.

## 4. A practical decision workflow

```text
Start with the question
        |
        v
What do you want to show?
  |        |        |         |
Compare   Trend   Distribution Relationship
  |        |        |         |
 Bar     Line    Histogram   Scatter
 Dot     Area*   ECDF/Box    Hexbin/Density*
        |
        v
Composition, geography, uncertainty, or flow?
        |
        v
Choose a specialized chart, label it clearly,
check scale/context, and validate interpretation.
```

`*` Use area charts carefully; use density/hexbin when point overlap makes a scatter plot hard to read.

## 5. Counts, totals, and rates

A chart can mislead when it compares counts without accounting for different group sizes. For example, a city with more incidents may also have a much larger population. Depending on the question, a rate per 1,000 residents may be more informative than the raw count.

Always define the denominator for percentages and rates. Do not compare percentages with different or unclear denominators.

## 6. Avoid common chart-selection mistakes

- Using a pie chart with many similar-sized categories.
- Using a line chart for unordered categories.
- Using a histogram for categorical labels.
- Comparing distributions with only means when spread and outliers matter.
- Using a dual-axis chart that encourages misleading visual comparisons.
- Using a truncated bar-chart axis that exaggerates differences.
- Comparing raw counts when group sizes differ substantially.
- Showing a 3D effect that distorts visual perception.
- Adding too many series, colors, or labels.
- Treating correlation or a fitted line as proof of causation.
- Omitting time range, units, sample size, source, or uncertainty.
- Choosing a complex chart that the intended audience cannot interpret.

## 7. Accessibility and communication

- Use descriptive titles and axis labels with units.
- Use readable type sizes and sufficient contrast.
- Do not rely on color alone; use labels, markers, or line styles too.
- Keep legends concise and place them where they do not obscure data.
- Order categories logically or by value when appropriate.
- Add notes about missing data, uncertainty, and limitations.
- Use the simplest chart that answers the question well.

## 8. Mini-project: Select charts for a business report

A business team wants to understand monthly revenue, product performance, order-value spread, advertising spend versus revenue, and revenue composition by channel.

For each question:

1. Identify the analytical goal and variable types.
2. Choose a chart and justify the choice.
3. Build the chart in `examples.ipynb`.
4. State one finding and one limitation.
5. Review the chart for misleading scales, missing context, and accessibility.

## Files in this module

- [`README.md`](README.md) — chart selection concepts and quick reference.
- [`examples.ipynb`](examples.ipynb) — worked examples and a decision workflow.
- [`practice.md`](practice.md) — progressive exercises.
- [`solutions.md`](solutions.md) — suggested answers and code examples.

## Further reading

- [ONE Data: How to choose the right chart](https://docs.one.org/guidelines/data-visualisation-guidelines/guidelines/chart-selection/)
- [Seaborn user guide and tutorial](https://seaborn.pydata.org/tutorial.html)
- [Matplotlib documentation](https://matplotlib.org/stable/)
- [Data-to-Viz chart selection guide](https://www.data-to-viz.com/)

## Completion checklist

- [ ] I can identify the main goal behind a chart request.
- [ ] I can select a chart for comparison, trend, distribution, relationship, and composition.
- [ ] I can explain limitations of common chart types.
- [ ] I can identify misleading scales and denominator problems.
- [ ] I can justify chart choices for a specific audience.
- [ ] I completed the practice exercises and mini-project.
