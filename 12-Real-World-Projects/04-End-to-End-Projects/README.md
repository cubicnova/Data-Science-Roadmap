# 04 — End-to-End Data Science Projects

This module brings the full data-science lifecycle together: business framing, data acquisition, validation, exploratory analysis, feature engineering, modeling, evaluation, packaging, deployment planning, monitoring, and communication.

The goal is to build projects that another person can reproduce and a stakeholder can understand—not just notebooks that produce a score.

## Learning objectives

By completing this module, you should be able to:

- Translate a business problem into a measurable target and decision.
- Design a reproducible repository and data pipeline.
- Validate schemas, data quality, and target timing.
- Separate training, validation, and test data to prevent leakage.
- Compare a baseline with a production-minded modeling pipeline.
- Evaluate models using metrics appropriate to the decision.
- Package reusable preprocessing and prediction code.
- Design an API and deployment workflow.
- Plan monitoring, drift detection, rollback, privacy, and cost controls.
- Communicate results, assumptions, and limitations clearly.

## Files in this module

- `README.md` — lifecycle, architecture, project specifications, and checklist.
- `examples.ipynb` — runnable end-to-end classification workflow using synthetic data.
- `practice.md` — progressive tasks and capstone requirements.
- `solution.md` — reference implementations, expected reasoning, and review criteria.

## Capstone options

Choose one primary project. The notebook uses synthetic customer-churn data so it can run locally without downloading a dataset.

| Project | Business goal | Model/task | Potential deliverable |
|---|---|---|---|
| Customer churn decision system | Prioritize retention outreach | Classification | Batch scoring pipeline, API plan, monitoring plan |
| House-price estimation service | Estimate a price range from property features | Regression | Reproducible model pipeline and prediction API |
| Retail demand forecasting | Estimate future product demand | Time-series forecasting | Backtesting report and forecast service |
| Customer segmentation workflow | Create useful behavior-based segments | Clustering | Segment profiles and a refresh process |
| Marketing performance analytics | Compare campaign performance | Statistical analysis / experimentation | KPI dashboard and decision memo |

## End-to-end lifecycle

1. **Problem definition**
   - Identify the decision, stakeholder, prediction time, target, time horizon, and success criteria.
   - Define what the system is and is not intended to do.

2. **Data acquisition and governance**
   - Record source, ownership, access approval, refresh cadence, and retention.
   - Use synthetic or approved datasets for practice.
   - Minimize sensitive fields and keep secrets outside source control.

3. **Data validation**
   - Check required columns, data types, missingness, duplicates, invalid ranges, and target distribution.
   - Fail clearly when critical schema assumptions are violated.
   - Track data-quality failures rather than silently ignoring them.

4. **Exploratory data analysis**
   - Examine distributions, relationships, segment sizes, outliers, and missingness.
   - Distinguish descriptive findings from causal claims.
   - Record findings and caveats in plain language.

5. **Feature engineering**
   - Use only information available at the prediction time.
   - Fit learned transformations on training data only.
   - Keep transformations in a reusable pipeline.

6. **Modeling**
   - Start with a simple baseline.
   - Compare candidate models with consistent splits and metrics.
   - Use cross-validation or time-aware validation where appropriate.

7. **Evaluation and decision policy**
   - Select metrics that match the business impact.
   - Inspect confusion matrices or residuals and evaluate meaningful subgroups.
   - Set thresholds based on costs, capacity, and validation data—not intuition alone.

8. **Packaging**
   - Save the preprocessing-plus-model pipeline as one versioned artifact.
   - Record feature schema, package versions, training data version, and evaluation results.
   - Never load untrusted serialized model files.

9. **Serving and deployment**
   - Expose a batch job or API with validation and useful errors.
   - Containerize if needed; externalize configuration and secrets.
   - Use staged releases, health checks, least privilege, and rollback.

10. **Monitoring and maintenance**
    - Monitor data quality, schema changes, latency, error rate, resource usage, and prediction distributions.
    - Track delayed outcome metrics when labels become available.
    - Define alert ownership, retraining criteria, and rollback conditions.

11. **Communication**
    - Deliver a README, data dictionary, model card, evaluation report, and concise recommendation.
    - State limitations and what evidence is needed before broad rollout.

## Reference architecture

```text
Approved data source
        |
        v
Data validation -> Data quality report
        |
        v
EDA + feature engineering
        |
        v
Train / validate / test split
        |
        v
Baseline -> candidate pipeline -> evaluation
        |
        v
Versioned model artifact + model card
        |
        +---- Batch scoring job
        |
        +---- Prediction API / container
                    |
                    v
         Logs, metrics, drift checks
                    |
                    v
          Alert / review / retraining
```

## Suggested repository structure

```text
customer-churn-end-to-end/
├── README.md
├── requirements.txt
├── data/
│   └── README.md
├── notebooks/
│   └── exploration.ipynb
├── src/
│   ├── __init__.py
│   ├── data_validation.py
│   ├── train.py
│   ├── predict.py
│   └── api.py
├── tests/
│   ├── test_data_validation.py
│   └── test_predict.py
├── models/
│   └── README.md
├── reports/
│   ├── evaluation.md
│   └── model_card.md
├── Dockerfile
└── .gitignore
```

Keep large data and model artifacts out of Git unless the repository policy explicitly permits them. Document approved acquisition and generation steps instead.

## Setup

```bash
python -m venv .venv
# Windows PowerShell
.venv\Scripts\Activate.ps1
# macOS/Linux
source .venv/bin/activate

python -m pip install --upgrade pip
pip install numpy pandas matplotlib scikit-learn joblib jupyter
jupyter notebook
```

The example notebook generates its own synthetic data. It does not provision cloud infrastructure or contact external services.

## Project requirements

A complete submission should include:

- **Problem brief:** stakeholder, decision, target, prediction time, success criteria.
- **Data documentation:** source or generator, data dictionary, schema, limitations.
- **Validation:** missingness, duplicates, invalid values, target balance, leakage review.
- **EDA:** readable charts, observations, and caveats.
- **Modeling:** baseline, pipeline, reproducible split, and model comparison.
- **Evaluation:** suitable metrics, error analysis, and decision threshold rationale.
- **Packaging:** reusable prediction function and versioned artifact.
- **Serving plan:** batch or API interface, input schema, failure behavior, and configuration.
- **Operations:** monitoring signals, alerts, rollback, retraining policy, and cost assumptions.
- **Communication:** README, model card, and stakeholder summary.

## Evaluation rubric (100 points)

| Area | Points | Evidence |
|---|---:|---|
| Problem definition and success criteria | 10 | Target, prediction time, action, measurable outcome |
| Data governance and validation | 15 | Source, schema, quality checks, privacy, leakage review |
| EDA and feature engineering | 10 | Useful findings and training-only transformations |
| Modeling and reproducibility | 15 | Baseline, pipeline, seed, versioned dependencies |
| Evaluation and error analysis | 15 | Appropriate metrics, holdout discipline, subgroup review |
| Packaging and interface | 10 | Reusable pipeline, schema validation, useful errors |
| Deployment and monitoring design | 10 | Security, health, metrics, rollback, cost |
| Communication and limitations | 10 | Clear report, model card, realistic recommendation |
| Tests and maintainability | 5 | Automated tests, structure, instructions |

## Definition of done

- [ ] Another person can reproduce the analysis from the instructions.
- [ ] The data source or synthetic generation assumptions are documented.
- [ ] Schema and data-quality checks run before modeling.
- [ ] No target leakage is knowingly present.
- [ ] Preprocessing is fitted only on training data.
- [ ] The final test set was not repeatedly used to tune the model.
- [ ] Metrics match the intended decision.
- [ ] Prediction code validates input and returns clear errors.
- [ ] Model artifact and dependencies are versioned/documented.
- [ ] Monitoring, rollback, privacy, and cost are addressed.
- [ ] The report states limitations and avoids unsupported causal claims.

## Important note

The included churn example is an educational demonstration with synthetic data. Its metrics do not represent real customer behavior, and it should not be used to make decisions about actual people.
