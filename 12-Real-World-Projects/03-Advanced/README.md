# Advanced Real-World Data Science Projects

This module is designed for portfolio projects that go beyond basic notebooks. Focus on robust validation, reproducible pipelines, model interpretation, monitoring, and clear communication of risks and trade-offs.

## Learning objectives

By the end of this module, you should be able to:

- Frame a complex business or operational problem as a measurable ML task.
- Build reproducible preprocessing and modeling pipelines.
- Use appropriate validation methods and avoid leakage.
- Compare models with business-relevant metrics.
- Interpret model behavior and inspect errors across segments.
- Document limitations, risks, and deployment considerations.
- Produce a portfolio-ready project with a repeatable workflow.

## Advanced project ideas

| Project | Core challenge | Suggested methods | Deliverables |
|---|---|---|---|
| Customer Churn Prediction | Imbalanced outcomes and intervention prioritization | Cross-validation, threshold tuning, calibration, explainability | Model report and action analysis |
| Fraud or Anomaly Detection | Rare events and changing patterns | Precision-recall analysis, anomaly detection, cost-sensitive evaluation | Detection report and alert review |
| Demand Forecasting | Time-dependent data and seasonality | Time-based splits, lag features, baseline forecasts | Forecast evaluation and error analysis |
| Recommendation System | Ranking relevant items | Popularity baseline, collaborative filtering, ranking metrics | Offline evaluation and recommendation demo |
| NLP Text Classification | Noisy text and model quality trade-offs | TF-IDF baseline, linear classifier, error analysis | Reproducible text pipeline |
| Model Drift Investigation | Changing data and delayed labels | Reference/current comparisons, segment monitoring | Monitoring report and response plan |

## Recommended workflow

1. **Define success:** Specify the user, decision, target, constraints, and primary metric.
2. **Audit the data:** Confirm provenance, permissions, target quality, time coverage, missingness, and possible leakage.
3. **Create a baseline:** Use a simple model or business rule before adding complexity.
4. **Design validation:** Use stratified or grouped splits when appropriate; use chronological splits for time-dependent tasks.
5. **Build a pipeline:** Keep preprocessing, feature selection, and estimator fitting inside cross-validation.
6. **Compare candidates:** Evaluate relevant metrics, uncertainty, runtime, calibration, and operational cost.
7. **Analyze errors:** Inspect false positives/negatives and performance across important segments.
8. **Interpret cautiously:** Distinguish association from causation and document limitations.
9. **Prepare delivery:** Save artifacts, metadata, metrics, and instructions; add tests and a clear report.
10. **Plan operations:** Define monitoring, retraining criteria, rollback, and human review where needed.

## Setup

Python 3.10+ is recommended.

```bash
python -m venv .venv
# Windows:
.venv\\Scripts\\activate
# macOS/Linux:
source .venv/bin/activate

python -m pip install --upgrade pip
python -m pip install numpy pandas matplotlib scikit-learn jupyter
jupyter notebook
```

Open `examples.ipynb` and run cells from top to bottom. The notebook creates synthetic data and requires no external dataset download.

## Project quality standards

- Use a held-out test set only for final evaluation after model selection.
- Fit learned transformations on training folds only.
- Compare against a meaningful baseline.
- Report metrics with enough context to interpret them.
- Avoid hard-coding machine-specific paths or secrets.
- Record data and code versions, dependencies, random seeds, and model configuration.
- Include a reproducible command sequence and known limitations.
- Never commit private data, credentials, or untrusted serialized artifacts.

## Suggested structure

```text
03-Advanced/
├── README.md
├── examples.ipynb
├── practice.md
├── solution.md
├── reports/
└── src/
```

## Practice

Complete the exercises in [`practice.md`](practice.md), then compare your implementation with [`solution.md`](solution.md).

## References

- scikit-learn model evaluation: https://scikit-learn.org/stable/modules/model_evaluation.html
- scikit-learn pipelines: https://scikit-learn.org/stable/modules/compose.html
- scikit-learn cross-validation: https://scikit-learn.org/stable/modules/cross_validation.html
- scikit-learn inspection and permutation importance: https://scikit-learn.org/stable/inspection.html
