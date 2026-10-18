# Data Science Interview Case Studies

This module helps you structure open-ended data science interview problems from business framing through analysis, modeling, evaluation, and recommendations. It includes a runnable synthetic customer-churn case, practice prompts, and reference solutions.

## Learning objectives

- Clarify the business objective, decision, users, constraints, and success metric.
- Translate a business problem into a measurable data science task.
- Inspect data quality, target leakage, missingness, class imbalance, and sampling bias.
- Build a baseline before adding complexity.
- Select metrics based on business costs rather than accuracy alone.
- Explain validation strategy, threshold selection, interpretability, deployment, and monitoring.
- Communicate a concise recommendation with risks, assumptions, and next steps.

## Files

| File | Purpose |
|---|---|
| `README.md` | Case-study framework and interview guidance |
| `examples.ipynb` | End-to-end synthetic customer-churn case study |
| `practice.md` | Case prompts, follow-up questions, and exercises |
| `solution.md` | Worked solutions, sample code, and reasoning |

## A repeatable case-study framework

Use this structure for most data science case interviews:

1. **Clarify the decision:** Who will use the result, what action can they take, and what is the business objective?
2. **Define the target and unit:** Specify the prediction/analysis target, observation grain, and prediction horizon.
3. **Choose success metrics:** Include business KPIs and model metrics. Explain trade-offs and costs.
4. **Understand the data:** Identify sources, timestamps, labels, coverage, missingness, duplicates, and potential leakage.
5. **Create a baseline:** Start with a simple heuristic or interpretable model.
6. **Validate correctly:** Use train/validation/test splits that reflect the production setting; use time-based splits when predicting the future.
7. **Compare and diagnose:** Evaluate relevant metrics, error slices, calibration, fairness, and robustness.
8. **Translate into action:** Set an operating threshold or intervention policy and estimate business impact.
9. **Plan production:** Address serving, monitoring, retraining, ownership, privacy, and rollback.
10. **Communicate:** Present recommendation, evidence, limitations, and the next experiment.

## Case-study notebook

The notebook builds a synthetic churn dataset and demonstrates:
- Data inspection and exploratory analysis
- A stratified train/test split
- A preprocessing pipeline for numeric and categorical features
- Baseline and logistic-regression models
- ROC-AUC, precision, recall, F1, and confusion matrix
- Threshold analysis for a hypothetical retention campaign
- Permutation importance and a short business recommendation

The generated dataset is illustrative, not a real customer dataset. Results are not intended as production benchmarks.

## Common case-study types

- Customer churn and retention
- Fraud and anomaly detection
- Demand and sales forecasting
- Recommendation and ranking
- Marketing campaign measurement and A/B testing
- Search relevance and conversion optimization
- Credit-risk or default prediction
- Operations, delivery, and capacity optimization
- Data quality and metric-debugging investigations

## Interview communication tips

- State assumptions before coding; ask only the clarifying questions that affect the solution.
- Distinguish correlation from causation.
- Avoid claiming that a model causes an outcome without an experiment or credible causal design.
- Explain why the chosen metric matches the business objective.
- Check for leakage, selection bias, and train/test contamination.
- Report uncertainty and failure modes, not only the best score.
- Give a recommendation with measurable impact and a plan to validate it.

## Interview checklist

- [ ] Can I define the business decision and unit of analysis?
- [ ] Can I translate the request into a target, horizon, and success metric?
- [ ] Can I identify leakage and choose an appropriate split?
- [ ] Can I build a simple baseline and compare it fairly?
- [ ] Can I select metrics and thresholds using business costs?
- [ ] Can I explain segment-level errors and limitations?
- [ ] Can I design an experiment to validate an intervention?
- [ ] Can I describe production monitoring and retraining triggers?
- [ ] Can I deliver a clear executive summary in under two minutes?
