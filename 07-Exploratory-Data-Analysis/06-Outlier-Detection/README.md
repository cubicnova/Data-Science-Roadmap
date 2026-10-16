# 06 — Outlier Detection

> Identify unusual observations, investigate why they occur, and choose a defensible treatment without automatically discarding valuable data.

## Learning objectives
- Explain what an outlier is and why it matters during EDA.
- Distinguish statistical outliers from data-entry or measurement errors.
- Detect potential outliers with the Interquartile Range (IQR) rule and z-scores.
- Use box plots, histograms, and scatter plots to inspect unusual values.
- Understand why detection methods make different assumptions.
- Compare data before and after a documented treatment.
- Choose among keeping, correcting, removing, capping, or transforming observations.
- Avoid data leakage when outlier thresholds are used in machine-learning workflows.

## What is an outlier?
An outlier is an observation that differs substantially from the pattern of most observations. It may represent a data error, a rare but valid event, a special population, or a naturally heavy-tailed distribution. An algorithm can flag a value as unusual, but context is needed to decide what it means.

**Detection is not the same as treatment.** Flag observations first, investigate them, and only then decide what action is appropriate.

## Methods covered

### 1. Interquartile Range (IQR) rule

Let `Q1` be the 25th percentile and `Q3` the 75th percentile.

- `IQR = Q3 - Q1`
- `Lower fence = Q1 - 1.5 × IQR`
- `Upper fence = Q3 + 1.5 × IQR`

Values outside these fences are flagged as potential outliers. This method uses quartiles, but can still flag valid values in skewed distributions.

### 2. Z-score

A z-score expresses distance from the mean in standard deviation units:

`z = (x - mean) / standard_deviation`

A common screening threshold is `abs(z) > 3`. This can be misleading for skewed data, heavy tails, small samples, or data where extremes distort the mean and standard deviation. Treat it as a screening convention, not a universal law.

### 3. Visual and domain checks
- **Box plot:** shows quartiles, median, and points beyond the whisker rule.
- **Histogram/density plot:** reveals skewness, long tails, and multiple modes.
- **Scatter plot:** helps identify unusual combinations of two variables.
- **Domain rules:** check whether values are physically or logically possible.

## Recommended workflow
1. Define the variable and its expected valid range.
2. Inspect missing values, units, types, and summary statistics.
3. Visualize the distribution before selecting a method.
4. Flag candidates using a suitable statistical rule.
5. Inspect flagged rows and related fields.
6. Check the source or domain rules where possible.
7. Choose a treatment and document the rationale.
8. Compare summaries and visualizations before and after treatment.
9. Preserve the original data and keep an audit trail.
10. If modeling, fit thresholds on training data only and apply them to validation/test data.

## Treatment options

| Option | When it may be appropriate | Caution |
|---|---|---|
| Keep | The value is valid and meaningful, such as a genuine high transaction | Some summaries and models may be sensitive to it |
| Correct | The source confirms a typo or measurement issue and the correct value is known | Never guess a replacement without evidence |
| Remove | The record is invalid or outside the clearly defined target population | Document criteria and impact |
| Cap / winsorize | Extreme magnitudes destabilize an analysis and a justified cap is acceptable | May hide meaningful extremes |
| Transform | A valid variable is strongly skewed and the analysis benefits from transformation | Interpretation changes |
| Analyze separately | Extremes represent a meaningful subgroup or rare event | Ensure the subgroup is not overlooked |

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

Open `examples.ipynb` and run cells from top to bottom. It generates a synthetic transaction dataset with intentionally unusual values. No external file is required.

## Files in this module
| File | Purpose |
|---|---|
| `README.md` | Concepts, methods, workflow, and treatment options |
| `examples.ipynb` | Runnable IQR, z-score, and visualization examples |
| `practice.md` | Exercises and interpretation tasks |
| `solution.md` | Worked code and decision-making guidance |

## Good practices
- Do not delete values solely because a method labels them as outliers.
- Check units and data-entry rules before changing values.
- A value can be unusual in one variable but normal in a meaningful subgroup.
- Use robust summaries such as median and IQR when distributions are skewed.
- Report the number and percentage of flagged observations.
- Keep an untouched raw dataset and perform changes on a copy.
- Document the method, thresholds, rows affected, and treatment.
- Do not calculate preprocessing thresholds from the full dataset before splitting model data; this can leak information from the test set.

## Completion checklist
- [ ] I inspected distributions before flagging values.
- [ ] I applied the IQR rule and understood its fences.
- [ ] I applied a z-score threshold and considered its assumptions.
- [ ] I reviewed flagged records instead of removing them automatically.
- [ ] I compared the original and treated data.
- [ ] I documented a reason for each treatment decision.

## Further reading
- [pandas — Quantile](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.quantile.html)
- [Seaborn — Distributions](https://seaborn.pydata.org/tutorial/distributions.html)
- [Seaborn — Relational plots](https://seaborn.pydata.org/tutorial/relational.html)

---
Part of the [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap).
