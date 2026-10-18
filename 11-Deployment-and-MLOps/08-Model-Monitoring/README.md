# Model Monitoring for Machine Learning

Model monitoring helps determine whether a deployed model remains reliable as live data, user behavior, and the operating environment change. It complements offline evaluation with ongoing checks after deployment.

## Learning objectives

By the end of this module, you should be able to:

- Explain model, data, service, and business monitoring.
- Track prediction counts, latency, errors, and model-quality metrics.
- Detect input data quality problems and feature drift.
- Understand the difference between data drift and concept drift.
- Build a simple monitoring report with Python.
- Design alerts and a feedback loop for delayed ground-truth labels.

## What should be monitored?

| Monitoring area | Example signals | Why it matters |
|---|---|---|
| Service health | Availability, latency, error rate, throughput | A good model is not useful if the service is down |
| Input data quality | Missing values, schema changes, invalid ranges | Bad inputs can cause failures or unreliable predictions |
| Data drift | Changes in feature distributions | Live data may differ from training data |
| Prediction behavior | Class proportions, score distributions | Unexpected outputs may indicate a problem |
| Model quality | Accuracy, precision, recall, F1, calibration | Requires trustworthy ground-truth labels |
| Business outcomes | Conversion, fraud loss, review rate | Connects model behavior to real-world impact |

## Drift concepts

- **Data drift:** the distribution of model inputs changes between a reference dataset and current production data.
- **Concept drift:** the relationship between inputs and the target changes over time.
- **Prediction drift:** the distribution of predictions changes.
- **Performance degradation:** evaluation metrics worsen when reliable labels become available.

A drift alert does not automatically mean the model is wrong. Investigate changes in data collection, population, seasonality, upstream systems, and label availability before deciding to retrain.

## Setup

Python 3.10+ is recommended.

```bash
python -m venv .venv
# Windows:
.venv\\Scripts\\activate
# macOS/Linux:
source .venv/bin/activate

python -m pip install --upgrade pip
python -m pip install numpy pandas scikit-learn matplotlib jupyter
jupyter notebook
```

Open `examples.ipynb` and run the cells from top to bottom. It generates synthetic reference and simulated production data, so no external dataset is required.

## Monitoring workflow

1. Establish a baseline from training or validation data.
2. Define input schema, allowed ranges, and expected service behavior.
3. Collect production inputs, predictions, timestamps, and latency while respecting privacy rules.
4. Compare current data and predictions with the baseline.
5. When labels arrive, calculate model-quality metrics.
6. Alert on meaningful changes and investigate root causes.
7. Document decisions; retrain only after confirming that it is appropriate.
8. Evaluate candidate models before deployment and retain rollback capability.

## Example checks

This module demonstrates simple checks with pandas and scikit-learn:

- Missing-value rate and numeric summary statistics.
- A standardized mean-shift indicator for numeric features.
- Prediction distribution comparisons.
- Classification metrics when labels are available.
- A basic monitoring report saved to CSV/JSON.

These are educational diagnostics, not a complete production monitoring platform. Thresholds must be selected for the use case and validated against normal variation.

## Good practices

- Monitor service health separately from model quality.
- Track reference and current data windows, sample counts, and timestamps.
- Validate schemas before running statistical drift checks.
- Account for seasonality and segment-level changes.
- Avoid alerting on tiny samples or ordinary noise.
- Monitor important segments as well as aggregate metrics.
- Do not log secrets or unnecessary personal data; apply access controls and retention limits.
- Do not automatically retrain solely because a drift statistic crossed a threshold.
- Establish alert ownership, escalation, rollback, and incident review procedures.

## Repository structure

```text
08-Model-Monitoring/
├── README.md
├── examples.ipynb
├── practice.md
└── solution.md
```

## Practice

Complete the exercises in [`practice.md`](practice.md), then compare your work with [`solution.md`](solution.md).

## References

- Evidently documentation: https://docs.evidentlyai.com/
- scikit-learn model evaluation: https://scikit-learn.org/stable/modules/model_evaluation.html
- pandas documentation: https://pandas.pydata.org/docs/
