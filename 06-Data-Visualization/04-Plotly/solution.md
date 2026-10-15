# Plotly Practice — Suggested Solutions

These are example solutions, not the only valid answers. Run cells in order. Some optional features, such as trendlines, may require additional packages.

## 1–5. Load data and create a scatter plot

```python
import plotly.express as px
import pandas as pd

iris = px.data.iris()
iris.head()

fig = px.scatter(
    iris,
    x="sepal_width",
    y="sepal_length",
    color="species",
    hover_data=["petal_length", "petal_width"],
    title="Iris Species by Sepal Measurements",
    labels={
        "sepal_width": "Sepal width (cm)",
        "sepal_length": "Sepal length (cm)",
    },
)
fig.show()
```

## 6. Count observations by species

```python
species_counts = iris["species"].value_counts().rename_axis("species").reset_index(name="count")
fig = px.bar(
    species_counts,
    x="species",
    y="count",
    title="Number of Iris Observations by Species",
    labels={"species": "Species", "count": "Observations"},
    text="count",
)
fig.show()
```

## 7–8. Monthly sales line chart

```python
sales = pd.DataFrame({
    "Month": ["Jan", "Feb", "Mar", "Apr", "May", "Jun"],
    "Sales": [120, 150, 135, 180, 210, 195],
})

fig = px.line(
    sales, x="Month", y="Sales", markers=True,
    title="Monthly Sales",
    labels={"Month": "Month", "Sales": "Sales (units)"},
)
fig.show()
```

## 9. Histogram of total bills

```python
tips = px.data.tips()
fig = px.histogram(
    tips, x="total_bill", nbins=20,
    title="Distribution of Total Bills",
    labels={"total_bill": "Total bill ($)"},
)
fig.show()
```

## 10. Box plot by day and sex

```python
fig = px.box(
    tips, x="day", y="total_bill", color="sex",
    title="Total Bill Distribution by Day and Sex",
    labels={"day": "Day", "total_bill": "Total bill ($)", "sex": "Sex"},
)
fig.show()
```

## 11. Total bill versus tip

```python
fig = px.scatter(
    tips, x="total_bill", y="tip", color="time",
    hover_data=["day", "size"],
    title="Tip Amount vs Total Bill",
    labels={"total_bill": "Total bill ($)", "tip": "Tip ($)", "time": "Meal"},
)
fig.show()

# Optional trendline (requires statsmodels):
# fig = px.scatter(tips, x="total_bill", y="tip", trendline="ols")
# fig.show()
```

## 12. Share of total bills by day

```python
bill_by_day = tips.groupby("day", as_index=False)["total_bill"].sum()
fig = px.pie(
    bill_by_day, names="day", values="total_bill",
    title="Share of Total Bill Value by Day",
)
fig.show()
```

## 13. Correlation heatmap

```python
numeric_tips = tips.select_dtypes(include="number")
corr = numeric_tips.corr()

fig = px.imshow(
    corr, text_auto=".2f", aspect="auto",
    color_continuous_scale="RdBu_r", zmin=-1, zmax=1,
    title="Correlation Matrix for Tips Dataset",
)
fig.show()
```

## 14. Average tip by day, sorted

```python
avg_tip = tips.groupby("day", as_index=False)["tip"].mean().sort_values("tip", ascending=False)
fig = px.bar(
    avg_tip, x="day", y="tip", text_auto=".2f",
    title="Average Tip by Day",
    labels={"day": "Day", "tip": "Average tip ($)"},
)
fig.show()
```

## 15–17. Template, labels, and hover information

```python
fig = px.scatter(
    tips, x="total_bill", y="tip", color="day",
    hover_data=["sex", "smoker", "time", "size"],
    labels={"total_bill": "Bill ($)", "tip": "Tip ($)"},
    title="Bills and Tips",
    template="plotly_white",
)
fig.show()
```

## 18–19. Graph Objects bar chart with multiple traces

```python
import plotly.graph_objects as go

fig = go.Figure()
fig.add_trace(go.Bar(name="2025", x=["A", "B", "C"], y=[12, 18, 15]))
fig.add_trace(go.Bar(name="2026", x=["A", "B", "C"], y=[16, 14, 21]))
fig.update_layout(
    title="Category Values: 2025 vs 2026",
    xaxis_title="Category",
    yaxis_title="Value",
    barmode="group",
)
fig.show()
```

## 20. Export to interactive HTML

```python
fig.write_html("plotly_chart.html", include_plotlyjs=True)
```

The saved file can be opened in a browser. Including Plotly.js makes the HTML file self-contained, though it is larger.

## 21. Faceted scatter plot

```python
fig = px.scatter(
    iris, x="sepal_width", y="sepal_length",
    color="species", facet_col="species",
    title="Sepal Measurements by Species",
)
fig.show()
```

## 22. 3D scatter plot

```python
fig = px.scatter_3d(
    iris,
    x="sepal_length", y="sepal_width", z="petal_length",
    color="species",
    title="Iris Measurements in 3D",
)
fig.show()
```

## 23. Animated Gapminder scatter plot

```python
gap = px.data.gapminder()
fig = px.scatter(
    gap,
    x="gdpPercap", y="lifeExp",
    animation_frame="year", animation_group="country",
    size="pop", color="continent", hover_name="country",
    log_x=True, size_max=50,
    range_x=[100, 100000], range_y=[20, 90],
    title="GDP per Capita and Life Expectancy Over Time",
    labels={"gdpPercap": "GDP per capita (international $)", "lifeExp": "Life expectancy (years)"},
)
fig.show()
```

## 24. Two-panel subplot

```python
from plotly.subplots import make_subplots
import plotly.graph_objects as go

fig = make_subplots(
    rows=1, cols=2,
    subplot_titles=("Monthly Sales", "Monthly Costs"),
)
months = ["Jan", "Feb", "Mar", "Apr"]
fig.add_trace(go.Scatter(x=months, y=[10, 14, 13, 18], mode="lines+markers", name="Sales"), row=1, col=1)
fig.add_trace(go.Bar(x=months, y=[7, 9, 8, 11], name="Costs"), row=1, col=2)
fig.update_layout(title_text="Sales and Costs", showlegend=True)
fig.show()
```

## 25. Example interpretation

For a scatter plot of total bill versus tip, a common visible pattern is that larger bills often correspond to larger tips. This is an association, not proof that bill size alone causes the tip to increase. Consider group size, meal context, and other variables before drawing stronger conclusions.

## Reflection — Key Points

1. **Plotly Express** is a concise high-level API; **Graph Objects** provides detailed control over traces and layouts.
2. A **trace** is a plotted data series or visual layer.
3. A histogram shows the distribution of a numeric variable; a bar chart compares categories or aggregated values.
4. Interactivity helps users explore data, but can hide key information if labels, defaults, or tooltips are unclear.
5. Fixed animation ranges prevent axes from shifting between frames and make comparisons more consistent.
6. Pie charts become difficult to compare when there are many categories or similar-sized slices.
7. Tooltips should add useful context, names, units, and relevant values without overwhelming the viewer.
