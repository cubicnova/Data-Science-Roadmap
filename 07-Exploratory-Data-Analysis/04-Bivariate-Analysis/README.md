# 04 — Bivariate Analysis

> Explore the relationship between two variables using summaries, visualizations, and statistical measures.

## Learning objectives

By the end of this module, you should be able to:

- Explain the purpose of bivariate analysis in Exploratory Data Analysis (EDA).
- Choose methods based on whether the two variables are numerical, categorical, or mixed.
- Visualize numerical–numerical relationships with scatter plots and trend lines.
- Compare a numerical variable across categories using box plots and grouped summaries.
- Analyze two categorical variables with cross-tabulations and stacked/count plots.
- Interpret covariance and Pearson/Spearman correlation appropriately.
- Distinguish association from causation and identify confounding factors.
- Communicate findings with evidence and limitations.

## What is bivariate analysis?

Bivariate analysis examines two variables together to understand whether and how they are associated. It extends univariate analysis, which examines one variable at a time.

## Select a method for the variable types

| Pair of variables | Useful summaries | Common visualizations / methods |
|---|---|---|
| Numerical + numerical | Covariance, Pearson correlation, Spearman correlation | Scatter plot, regression line, joint plot |
| Numerical + categorical | Group means/medians, spread, group counts | Box plot, violin plot, strip plot, grouped bar chart |
| Categorical + categorical | Cross-tabulation, row/column percentages | Count plot, stacked bar chart, contingency-table analysis |

## Core concepts

- **Covariance** describes whether two numerical variables tend to move together. Its magnitude depends on the units of the variables.
- **Pearson correlation** measures the strength and direction of a linear association and ranges from -1 to +1.
- **Spearman correlation** measures monotonic association using ranks and can be useful when relationships are not linear or are affected by outliers.
- **Cross-tabulation** counts combinations of two categorical variables.
- **Group summaries** compare a numerical variable across categories.
- **Confounding** occurs when another variable influences or helps explain an observed association.
- **Association is not causation.** Two variables may move together without one causing the other.

## Workflow

1. Define the question about the two variables.
2. Confirm their meanings, types, units, and valid values.
3. Inspect missingness and sample size for the selected pair.
4. Choose a suitable table, statistic, or chart.
5. Check the shape of the relationship, outliers, and possible nonlinearity.
6. Compare relevant groups or compute association measures.
7. Consider confounding, sampling bias, and other limitations.
8. Write a conclusion supported by the analysis without overstating causality.

## Setup

```bash
python -m venv .venv
# Windows:
.venv\\Scripts\\activate
# macOS/Linux:
source .venv/bin/activate

python -m pip install pandas numpy matplotlib seaborn jupyter
jupyter notebook
```

Open `examples.ipynb` and run the cells from top to bottom. The notebook creates a reproducible synthetic student dataset in memory; no external download is required.

## Files in this module

| File | Purpose |
|---|---|
| `README.md` | Concepts, methods, workflow, and setup |
| `examples.ipynb` | Guided, runnable bivariate analysis notebook |
| `practice.md` | Exercises from beginner to challenge level |
| `solution.md` | Worked solutions and interpretation guidance |

## Good practices

- Plot numerical relationships before relying on a single coefficient.
- A Pearson correlation near zero does not rule out a nonlinear relationship.
- Correlation can be affected by outliers, restricted ranges, and subgroup structure.
- Report sample size and missing-value handling where relevant.
- For categorical comparisons, include counts alongside percentages.
- Avoid comparing group averages without considering group sizes and spread.
- Do not interpret a high correlation as proof of causation.
- Be careful with p-values: statistical significance does not automatically mean a relationship is practically important.

## Completion checklist

- [ ] I identified the types of both variables.
- [ ] I selected an appropriate chart or table.
- [ ] I inspected missing values and sample size.
- [ ] I calculated relevant summaries or association measures.
- [ ] I checked for outliers and nonlinear patterns.
- [ ] I explained findings, caveats, and why association is not causation.

## Further reading

- [Seaborn — Visualizing statistical relationships](https://seaborn.pydata.org/tutorial/relational.html)
- [Seaborn — Visualizing distributions](https://seaborn.pydata.org/tutorial/distributions.html)
- [pandas — Descriptive statistics](https://pandas.pydata.org/docs/user_guide/basics.html#descriptive-statistics)
- [SciPy — Statistical functions](https://docs.scipy.org/doc/scipy/reference/stats.html)

---
Part of the [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap).
