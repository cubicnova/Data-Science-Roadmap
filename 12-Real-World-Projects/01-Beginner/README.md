# Beginner Real-World Data Science Projects

This module introduces end-to-end beginner projects that turn basic Python, pandas, visualization, and machine-learning skills into practical portfolio work. Each project should have a clear question, a reproducible workflow, and a concise explanation of results.

## Learning objectives

By the end of this module, you should be able to:

- Translate a practical problem into a data-science question.
- Inspect, clean, and summarize a dataset.
- Use exploratory data analysis (EDA) to find patterns.
- Build a simple baseline model when prediction is appropriate.
- Evaluate results using metrics that fit the problem.
- Communicate findings with charts, limitations, and next steps.
- Organize a project so another person can reproduce it.

## Recommended beginner projects

| Project | Problem statement | Core skills | Suggested output |
|---|---|---|---|
| Student Performance Analysis | Which study and attendance patterns are associated with scores? | pandas, EDA, visualization, correlation | Analysis report and charts |
| Retail Sales Analysis | Which products and periods contribute most to sales? | data cleaning, grouping, time-based summaries | Sales dashboard/report |
| Customer Churn Exploration | Which customer attributes are associated with churn? | classification, train/test split, metrics | Baseline churn model |
| House Price Prediction | Can property attributes predict a sale price? | regression, preprocessing, MAE/RMSE | Regression baseline |
| Movie Ratings Analysis | Which genres and release periods have different rating patterns? | joins, aggregation, visualization | Storytelling notebook |

Use a dataset that is public and appropriately licensed, or generate synthetic data for practice. Do not upload personal or confidential data to a public repository.

## Project workflow

1. **Define the question:** Write a one-sentence question and explain why it matters.
2. **Understand the data:** Record the source, license, rows, columns, types, and target (if any).
3. **Clean carefully:** Check missing values, duplicates, invalid values, and data types. Explain important choices.
4. **Explore:** Use summary statistics and clear charts to investigate distributions and relationships.
5. **Establish a baseline:** For prediction tasks, start with a simple baseline before trying complex models.
6. **Evaluate appropriately:** Use a held-out test set and metrics suited to the task.
7. **Interpret:** Separate observations from causal claims; correlations do not prove causation.
8. **Communicate:** Summarize findings, limitations, and recommended next steps.
9. **Make it reproducible:** Include environment instructions, random seeds, and clear relative paths.

## Setup

Python 3.10+ is recommended.

```bash
python -m venv .venv
# Windows:
.venv\\Scripts\\activate
# macOS/Linux:
source .venv/bin/activate

python -m pip install --upgrade pip
python -m pip install numpy pandas matplotlib seaborn scikit-learn jupyter
jupyter notebook
```

Open `examples.ipynb` and run cells from top to bottom. The notebook uses generated synthetic data, so no external download is required.

## Suggested project structure

```text
project-name/
├── README.md
├── data/
│   └── README.md          # document source; avoid committing large/private data
├── notebooks/
│   └── analysis.ipynb
├── src/
│   └── analysis.py
├── reports/
│   └── figures/
├── requirements.txt
└── .gitignore
```

For a small learning project, a single notebook is fine. As a project grows, separate reusable logic from exploration.

## Portfolio checklist

- [ ] Clear problem statement and intended audience
- [ ] Data source and license documented
- [ ] Cleaning decisions explained
- [ ] Charts have titles, axes, units, and readable labels
- [ ] Baseline and evaluation method documented (if predictive)
- [ ] No target leakage or inappropriate use of test data
- [ ] Limitations and ethical considerations included
- [ ] Setup steps work from a clean environment
- [ ] No credentials, private data, or generated environment folders committed

## Practice

Complete the tasks in [`practice.md`](practice.md), then review the example solutions in [`solution.md`](solution.md).

## References

- pandas getting started: https://pandas.pydata.org/docs/getting_started/
- scikit-learn getting started: https://scikit-learn.org/stable/getting_started.html
- Matplotlib tutorials: https://matplotlib.org/stable/tutorials/
- Seaborn tutorial: https://seaborn.pydata.org/tutorial.html
