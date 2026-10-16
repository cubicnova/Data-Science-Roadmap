# 08 — EDA Case Studies

> Apply the full Exploratory Data Analysis workflow to realistic, scenario-based datasets and turn raw data into evidence-based recommendations.

## Learning objectives

By completing this module, you should be able to:

- Translate a business question into an EDA plan.
- Inspect dataset structure, variable types, missing values, and duplicate records.
- Perform univariate and bivariate analysis using summaries and visualizations.
- Identify anomalies and assess data-quality issues without blindly deleting observations.
- Compare groups, examine relationships, and formulate hypotheses for further analysis.
- Communicate findings with clear charts, numerical evidence, limitations, and next steps.
- Build a reproducible notebook with a clear narrative from question to recommendation.

## What is an EDA case study?

A case study combines data understanding, data-quality assessment, analysis, visualization, and communication around a specific question. Instead of running every possible chart, analysts choose methods that help answer the question and validate their findings.

## Case studies included

### Case Study 1 — Retail Sales Performance
Questions:
- Which product categories and regions contribute most to sales?
- How do order values vary?
- Are there unusual transaction values or missing fields?
- What additional information would help explain sales differences?

### Case Study 2 — Student Performance
Questions:
- How are study hours, attendance, and exam scores distributed?
- How do score distributions vary by department or study mode?
- Are study hours and exam scores associated in this sample?
- What confounders and limitations prevent causal conclusions?

### Case Study 3 — Customer Churn Exploration
Questions:
- How are tenure, monthly charges, and churn status distributed?
- Do observed customer characteristics differ between churn groups?
- Which patterns are useful to investigate further?
- Why are descriptive group differences not proof of causation?

The notebook generates small synthetic datasets in memory, so no external downloads are required. The data are for learning and are not real customer, sales, or student records.

## Recommended case-study workflow

1. **Business understanding:** write the problem, audience, and decisions the analysis may support.
2. **Data understanding:** inspect rows, columns, types, sample records, and summary statistics.
3. **Data quality:** quantify missingness, duplicates, invalid values, and unusual observations.
4. **Univariate analysis:** understand individual variable distributions and frequencies.
5. **Bivariate analysis:** compare pairs of variables relevant to the question.
6. **Visualization:** select clear charts that support specific findings.
7. **Interpretation:** separate observations from possible explanations.
8. **Recommendations:** propose next steps supported by the evidence.
9. **Limitations:** describe sample size, synthetic data, missing fields, and causal limits.
10. **Reproducibility:** keep code organized and make the notebook runnable from top to bottom.

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

Open `examples.ipynb` and run all cells from top to bottom.

## Files in this module

| File | Purpose |
|---|---|
| `README.md` | Case-study briefs, workflow, and reporting guidance |
| `examples.ipynb` | Three end-to-end EDA case studies |
| `practice.md` | Case-study tasks and extension challenges |
| `solution.md` | Worked approaches, code patterns, and reporting templates |

## Suggested final report structure

1. **Title and objective**
2. **Dataset overview** — row count, column count, variable types, and scope
3. **Data quality** — missingness, duplicates, invalid values, and anomalies
4. **Key findings** — three to five claims supported by numbers or charts
5. **Visual evidence** — well-labeled plots with concise captions
6. **Recommendations** — actionable next steps linked to findings
7. **Limitations** — data gaps, uncertainty, and why association is not causation
8. **Appendix** — important assumptions and reproducibility notes

## Good practices

- Start with a question, not a chart.
- Keep the raw data unchanged; use a separate working copy for cleaning.
- Include counts and denominators when comparing percentages.
- Check whether group sizes differ before comparing averages.
- Investigate outliers before removing or capping them.
- Avoid implying causation from correlations or group differences.
- Do not report a result without checking the underlying calculation.
- Synthetic datasets demonstrate methods, not real-world business truths.

## Completion checklist

- [ ] I completed all three case studies.
- [ ] I documented the objective and the variables used.
- [ ] I assessed data quality before interpreting results.
- [ ] I used suitable univariate and bivariate methods.
- [ ] Every finding is supported by a statistic or chart.
- [ ] I included caveats and practical next steps.
- [ ] My notebook runs from top to bottom.

## Further reading

- [pandas — Getting started](https://pandas.pydata.org/docs/getting_started/index.html)
- [Seaborn tutorials](https://seaborn.pydata.org/tutorial.html)
- [Matplotlib tutorials](https://matplotlib.org/stable/tutorials/)
- [The Data Science Process: A Comprehensive Guide](https://www.ibm.com/think/topics/data-science-process)

---
Part of the [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap).
