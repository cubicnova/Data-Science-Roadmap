# 02 — Intermediate Real-World Projects

Build portfolio-ready projects that combine data cleaning, exploratory analysis, feature engineering, machine learning, evaluation, interpretation, and communication.

This folder is a **project track**, not a single algorithm lesson. The projects use small synthetic datasets generated in the notebook, so you can run the examples without downloading private or external data.

## Learning outcomes

By completing this track, you should be able to:

- Translate a business question into a measurable data-science problem.
- Create a reproducible analysis workflow and document assumptions.
- Clean data while avoiding target leakage.
- Build preprocessing and modeling pipelines.
- Compare against a simple baseline before tuning complex models.
- Choose metrics that match the business problem.
- Explain findings and limitations to a non-technical audience.
- Package a project so another person can reproduce it.

## Files in this module

- `README.md` — project briefs, workflow, quality standards, and rubric.
- `examples.ipynb` — runnable examples with generated data and end-to-end modeling.
- `practice.md` — progressive exercises and project extensions.
- `solution.md` — reference solutions, implementation patterns, and review checklist.

## Recommended project sequence

| Project | Business question | Core skills | Difficulty |
|---|---|---|---|
| 1. Customer churn prediction | Which customers may cancel next month? | Classification, class imbalance, recall/precision, leakage prevention | Intermediate |
| 2. House price estimation | What price range is reasonable for a property? | Regression, categorical/numeric preprocessing, MAE/RMSE, residuals | Intermediate |
| 3. Customer segmentation | Which customer groups need different marketing strategies? | Scaling, clustering, PCA interpretation, segment profiling | Intermediate |
| 4. Sales performance analysis | What drives sales changes across products and regions? | Time-aware aggregation, visualization, business KPIs, cautious forecasting | Intermediate |
| 5. Marketing campaign analysis | Which campaigns appear to perform best? | Conversion rates, uncertainty, segment analysis, experiment caveats | Intermediate |

Start with Project 1 in the notebook. Then use the project framework and exercises to complete the remaining projects.

## Standard project lifecycle

1. **Business understanding:** state the decision, user, time horizon, and success metric.
2. **Data audit:** inspect row count, schema, missingness, duplicates, target definition, and possible leakage.
3. **Data preparation:** split data before fitting learned transformations; keep preprocessing inside a pipeline.
4. **EDA:** summarize distributions, segments, relationships, and data-quality concerns.
5. **Baseline:** establish a simple rule or model that a more complex approach must beat.
6. **Modeling:** fit an appropriate model and compare it with the baseline.
7. **Evaluation:** use a holdout set that reflects the real use case; select metrics before tuning.
8. **Interpretation:** inspect coefficients, feature importance, errors, and subgroup behavior.
9. **Recommendation:** translate results into an action, estimate trade-offs, and state uncertainty.
10. **Reproducibility:** record data provenance, random seed, dependencies, assumptions, and how to rerun.
11. **Delivery:** publish a clear README, charts/tables, model limitations, and next steps.

## Setup

Recommended environment:

```bash
python -m venv .venv
# Windows PowerShell
.venv\Scripts\Activate.ps1
# macOS/Linux
source .venv/bin/activate

python -m pip install --upgrade pip
pip install numpy pandas matplotlib scikit-learn jupyter
jupyter notebook
```

The notebook uses synthetic data and standard Python data-science packages. It is designed for learning, not for making real business decisions.

## Project 1 — Customer churn prediction

**Scenario:** a subscription service wants to identify customers at risk of cancelling within the next billing period.

**Target:** `churn` (1 = churn, 0 = retained).

**Possible features:** tenure, monthly fee, support tickets, contract type, payment method, and usage.

**Business framing:** a model score is not itself an intervention. Choose a threshold based on outreach cost, capacity, customer experience, and the relative cost of false negatives and false positives.

**Evaluation:** compare a majority-class baseline with logistic regression. Review precision, recall, F1, ROC-AUC, and the confusion matrix. If the target is imbalanced, accuracy alone is inadequate.

**Important risks:** target leakage, using information recorded after cancellation, unfairly treating correlations as causes, and contacting customers without a clear intervention plan.

## Project 2 — House price estimation

**Scenario:** estimate a property's sale price from its attributes.

- Define the prediction moment and ensure all features would be known at that time.
- Use a train/test split and a preprocessing pipeline.
- Compare a median-price baseline with a regression model.
- Report MAE in the target currency and inspect residuals by price range or location.
- Do not claim the model gives a formal appraisal or guarantee.

## Project 3 — Customer segmentation

**Scenario:** group customers to help design different engagement strategies.

- Select behavior features relevant to a decision.
- Scale numeric features before distance-based clustering.
- Compare a small range of cluster counts using several signals, including interpretability.
- Profile each cluster using original units and label segments descriptively.
- Clusters are exploratory patterns, not natural or permanent customer identities.

## Project 4 — Sales performance analysis

**Scenario:** sales leadership wants to understand changes by product, region, and month.

- Validate date, revenue, quantity, returns, and currency assumptions.
- Define gross sales, net sales, and margin explicitly.
- Aggregate at a consistent grain before comparing segments.
- Distinguish descriptive trends from causal explanations.
- Use time-based validation for any forecasting; never randomly mix future observations into training.

## Project 5 — Marketing campaign analysis

**Scenario:** compare campaign engagement and conversion.

- Define the denominator and attribution window.
- Compare conversion rates with sample sizes and uncertainty.
- Segment carefully; avoid over-interpreting small groups.
- If users were randomized, evaluate the experiment design and assignment process.
- Observational campaign data alone usually cannot establish causal lift.

## Suggested repository layout for an individual project

```text
customer-churn/
├── README.md
├── notebooks/
│   └── analysis.ipynb
├── src/
│   ├── data.py
│   └── modeling.py
├── tests/
├── reports/
│   └── figures/
├── requirements.txt
└── .gitignore
```

Do not commit credentials, private customer records, large generated files, or proprietary datasets. Describe how to obtain approved data and how to reproduce the analysis.

## Quality rubric (100 points)

| Area | Points | Evidence |
|---|---:|---|
| Business framing and target definition | 10 | Decision, user, target, time horizon |
| Data quality and leakage prevention | 15 | Schema, missing values, duplicates, split discipline |
| EDA and visual communication | 15 | Useful charts, written observations, caveats |
| Baseline and modeling | 15 | Sensible baseline, pipeline, reproducibility |
| Evaluation | 15 | Appropriate metrics and error analysis |
| Interpretation and recommendation | 10 | Actionable and appropriately cautious conclusions |
| Reproducibility and code quality | 10 | Clear structure, seed, requirements, instructions |
| Ethics, privacy, and limitations | 10 | Risk discussion, data minimization, subgroup checks |

## Completion checklist

- [ ] I can explain the business decision in one paragraph.
- [ ] I documented the target, prediction moment, and data provenance.
- [ ] I checked data quality and possible leakage.
- [ ] I used a baseline and an appropriate evaluation metric.
- [ ] All learned preprocessing was fit on training data only.
- [ ] I inspected errors and important limitations.
- [ ] I wrote a non-technical summary with a recommendation.
- [ ] Another person can reproduce the project from the README.

## Final advice

A strong intermediate project is not the one with the most complicated model. It is the one with a clear question, trustworthy evaluation, understandable findings, honest limitations, and a reproducible workflow.
