# 06 — Anomaly Detection

Anomaly detection identifies observations that differ substantially from expected patterns. It is used in fraud investigation, cybersecurity monitoring, equipment maintenance, quality control, and data validation. An anomaly is a signal for investigation—not automatically proof of an error or malicious activity.

## Learning objectives

By the end of this module, you should be able to:
- Distinguish outlier detection from novelty detection.
- Explain why anomaly detection often lacks reliable labels.
- Explore Isolation Forest, Local Outlier Factor (LOF), One-Class SVM, and robust covariance methods.
- Scale features appropriately and understand each model's assumptions.
- Interpret anomaly scores and tune alert thresholds.
- Evaluate labeled anomalies with precision, recall, F1, ROC-AUC, and average precision.
- Avoid leakage when selecting thresholds or evaluating models.
- Plan an operational workflow for reviewing and monitoring anomaly alerts.

## Key concepts

| Concept / algorithm | Main idea | Important caution |
|---|---|---|
| Outlier detection | Finds unusual observations in a dataset that may already contain anomalies | Training data may be contaminated |
| Novelty detection | Learns normal behavior from relatively clean data and flags unusual new observations | Normal behavior must be representative |
| Isolation Forest | Anomalies tend to be isolated in fewer random splits | Results depend on data and contamination assumptions |
| Local Outlier Factor (LOF) | Compares local density with neighboring observations | Sensitive to `n_neighbors`; novelty mode has different API behavior |
| One-Class SVM | Learns a boundary around normal observations | Scaling and parameter tuning are important; can be expensive |
| Elliptic Envelope | Uses robust covariance and an elliptical normal-data assumption | Unsuitable for strongly non-Gaussian or multimodal data |
| Contamination | Expected/provided proportion used to set a decision threshold in some estimators | It is not a guarantee of true anomaly prevalence |
| Anomaly score | Continuous measure of unusualness | Score direction and scale vary by estimator |

## Recommended workflow

1. Define what “anomalous” means in the domain and what action an alert should trigger.
2. Inspect missing values, feature scales, duplicate records, and time/group structure.
3. Separate training, validation, and test data appropriately; use time-based splits when future data are being predicted.
4. Fit on representative normal data if the task is novelty detection.
5. Compare multiple algorithms against a simple baseline.
6. Select thresholds on validation data based on review capacity and error costs.
7. Evaluate on held-out data, preferably with trusted labels or expert review.
8. Investigate false positives and false negatives; monitor drift and alert volume.
9. Reassess assumptions as normal behavior changes.

## Environment setup

Python 3.10+ is recommended.

```bash
python -m venv .venv
# Windows: .venv\\Scripts\\activate
# macOS/Linux: source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install numpy pandas matplotlib scikit-learn jupyter
jupyter notebook
```

Open `examples.ipynb` in Jupyter or VS Code. Examples use generated data and require no external dataset download.

## Files

- `README.md` — concepts, workflow, and setup.
- `examples.ipynb` — guided, runnable examples and visualizations.
- `practice.md` — progressive questions, coding exercises, and projects.
- `solution.md` — reference explanations and code patterns.

## Good practices

- Standardize features for distance- and kernel-based methods such as LOF and One-Class SVM.
- Fit preprocessing only on training data.
- Do not assume the model's default anomaly fraction is the real-world anomaly rate.
- Use the right LOF API: `fit_predict` for outlier detection, or `novelty=True` and predict only on unseen data for novelty detection.
- When labels exist, use precision-recall metrics and inspect confusion matrices at the chosen threshold.
- If labels are absent, combine expert review, alert-volume analysis, stability checks, and domain constraints.
- Do not automatically delete anomalous records; rare observations can be valid and important.
- Document score direction, threshold, model version, features, and review outcomes.

## Further reading

- [scikit-learn: Novelty and outlier detection](https://scikit-learn.org/stable/modules/outlier_detection.html)
- [scikit-learn: Model evaluation](https://scikit-learn.org/stable/modules/model_evaluation.html)
