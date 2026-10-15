# 04 - Plotly

Interactive data visualization with **Plotly in Python**.

This module introduces Plotly Express for quick charts and `plotly.graph_objects` for fine-grained control. Plotly charts support hover tooltips, zooming, panning, legends, and interactive HTML export.

## Learning Objectives

By the end of this module, you will be able to:

- Explain when interactive visualizations are useful.
- Create common charts with Plotly Express.
- Customize titles, axes, labels, colors, hover information, and legends.
- Use `plotly.graph_objects` to construct and modify figures.
- Create histograms, box plots, heatmaps, and 3D scatter plots.
- Build faceted and animated charts.
- Export interactive charts to HTML.
- Choose chart types that communicate data clearly and responsibly.

## Prerequisites

- Basic Python syntax and functions
- Pandas DataFrames
- Basic data visualization concepts
- Jupyter Notebook

## Installation

```bash
python -m pip install plotly pandas jupyter
```

Optional: install Kaleido and Chrome/Chromium if you need static image export.

```bash
python -m pip install -U kaleido
```

## Repository Contents

| File | Purpose |
|---|---|
| `README.md` | Concepts, setup, and learning path |
| `examples.ipynb` | Guided, runnable Plotly examples |
| `practice.md` | Exercises to complete independently |
| `solution.md` | Suggested solutions and explanations |

## Learning Path

1. **Plotly basics** — figures, traces, and layouts.
2. **Plotly Express** — create charts with concise functions.
3. **Chart selection** — scatter, line, bar, histogram, box, pie, and heatmap.
4. **Customization** — titles, axis labels, hover data, templates, and legends.
5. **Graph Objects** — add traces and control figure details.
6. **Advanced views** — facets, subplots, 3D charts, and animation.
7. **Exporting** — save interactive charts as HTML.

## Core Concepts

- **Figure:** the complete visualization, including data and layout.
- **Trace:** one plotted data series, such as a line or bar series.
- **Layout:** titles, axes, legend, margins, and other presentation settings.
- **Plotly Express (`px`):** a high-level API recommended for most everyday charts.
- **Graph Objects (`go`):** a lower-level API for more detailed figure construction.

## Quick Start

```python
import plotly.express as px

df = px.data.iris()
fig = px.scatter(
    df,
    x="sepal_width",
    y="sepal_length",
    color="species",
    title="Iris Dataset: Sepal Measurements",
    hover_data=["petal_length", "petal_width"],
)
fig.show()
```

## Recommended Practices

- Pick a chart based on the question you need to answer.
- Label axes and include meaningful units.
- Use hover data to provide context, not to hide essential information.
- Keep colors consistent and accessible.
- Avoid 3D charts when a 2D chart communicates the result more clearly.
- Check missing values, data types, and aggregation choices before plotting.
- For reproducibility, keep data preparation and plotting steps explicit.
- Treat interactive charts as a complement to, not a replacement for, clear titles and labels.

## Common Troubleshooting

- **`ModuleNotFoundError: No module named 'plotly'`:** install Plotly in the Python environment used by Jupyter.
- **Notebook chart does not render:** update Plotly and restart the kernel; try `fig.show()`.
- **Static image export fails:** install a compatible Kaleido version and Chrome/Chromium.
- **Too many points make a chart slow:** aggregate, filter, or sample the data; consider WebGL-based chart types where appropriate.

## Practice

Complete the exercises in [`practice.md`](practice.md), then compare your work with [`solution.md`](solution.md). Run the notebook from top to bottom.

## References

- [Plotly Python documentation](https://plotly.com/python/)
- [Plotly Express](https://plotly.com/python/plotly-express/)
- [Graph Objects](https://plotly.com/python/graph-objects/)
- [Subplots](https://plotly.com/python/subplots/)
- [Animations](https://plotly.com/python/animations/)
