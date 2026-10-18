# Practice — Anomaly Detection

Complete the exercises in order. Use Python, pandas, matplotlib, and scikit-learn. The synthetic datasets provide known labels for evaluation; in real deployments, labels may be incomplete or delayed.

**Suggested environment:** see `README.md`. Keep preprocessing and threshold selection separate from final evaluation.


## Foundations

1. Define anomaly detection and give three applications from different industries.
2. Distinguish outlier detection from novelty detection. When is each appropriate?
3. Explain point anomalies, contextual anomalies, and collective anomalies with examples.
4. Why is anomaly detection often treated as an unsupervised or semi-supervised task?
5. Explain why an anomalous observation is not necessarily an error or malicious event.
6. Describe the challenges of evaluating anomaly detection when ground-truth labels are scarce.

## Data preparation

7. Generate a two-dimensional dataset with normal observations and injected anomalies. Plot both groups.
8. Inspect feature distributions, missing values, and scale differences before fitting a detector.

## Isolation Forest

9. Fit `IsolationForest` and identify predicted anomalies. Explain the meaning of labels `1` and `-1`.
10. Vary `contamination` and compare the number of flagged points. Why is this parameter not a guarantee of true prevalence?
11. Compare `n_estimators` values and record runtime, flagged count, and stability.

## LOF

12. Use `LocalOutlierFactor` with `fit_predict` for outlier detection on the training dataset.
13. Fit LOF with `novelty=True` on clean training data and predict on unseen observations. Explain why using `predict` on training samples is discouraged in novelty mode.
14. Vary `n_neighbors` and describe how local-density assumptions affect the results.

## One-Class SVM

15. Scale the features and fit `OneClassSVM`. Explain the role of `nu` and `gamma`.
16. Compare One-Class SVM with and without scaling. Explain why feature scale matters.

## Robust covariance

17. Fit `EllipticEnvelope` to approximately Gaussian data. Explain its elliptical-distribution assumption.

## Algorithm comparison

18. Compare Isolation Forest, LOF, and One-Class SVM on the same generated data and evaluate them with known synthetic labels.

## Metrics

19. Calculate precision, recall, F1, ROC-AUC, and average precision when synthetic anomaly labels are available.
20. Explain why accuracy is a weak primary metric when anomalies are rare.

## Thresholds

21. Use a validation set to choose a threshold that meets a target recall. Evaluate the chosen threshold on test data only once.
22. Select a threshold under a limited alert-review budget. Explain how you would estimate precision at that alert volume.

## Score interpretation

23. Compare `decision_function` and `score_samples` for Isolation Forest. Verify which score direction corresponds to anomalies.

## Preprocessing

24. Build a `Pipeline` with `StandardScaler` and One-Class SVM. Explain how the pipeline helps avoid leakage.

## Time series

25. Describe how you would detect unusual daily transaction counts without randomly mixing future and past observations.

## Feature engineering

26. Create rolling mean, rolling standard deviation, and deviation-from-baseline features for a time series. Avoid using future information.

## Error analysis

27. Review flagged observations and categorize them as data errors, legitimate rare events, emerging behavior, or false alarms.

## Stability

28. Repeat a detector with multiple seeds or data samples. Measure overlap between flagged sets.

## High dimensions

29. Explain why distance and density become less informative in high-dimensional spaces. Suggest two mitigations.

## Contamination

30. Explain how contaminated training data can distort a novelty detector and propose a robust training strategy.

## Drift

31. Design a monitoring plan for changes in feature distributions, anomaly scores, alert volume, and analyst-confirmed outcomes.

## Cybersecurity

32. Design an anomaly-detection workflow for login events. Identify useful features, risks, and analyst feedback loops.

## Fraud

33. Design a transaction anomaly workflow and explain why anomaly scores should not be treated as confirmed fraud labels.

## Deployment

34. Specify latency, throughput, alert prioritization, audit logging, and fallback requirements for a production detector.

## Explainability

35. Create an analyst-facing explanation for an alert using feature deviations and context, without claiming causality.

## Mini-project

36. Build a complete synthetic anomaly-detection project: generate data, compare three algorithms, evaluate known labels, and visualize alerts.
37. Create a novelty-detection project trained only on normal examples, then evaluate on a separate dataset containing normal and anomalous points.

## Review

38. Write a model-selection memo comparing performance, assumptions, scalability, interpretability, and operational cost.
39. List five common anomaly-detection mistakes and explain how to prevent each.

## Challenge

40. Build a small alert triage report that ranks observations by anomaly score, records the selected threshold, and summarizes review capacity.

## Submission checklist

- [ ] State whether the task is outlier detection or novelty detection.
- [ ] Describe the expected normal behavior and anomaly definition.
- [ ] Compare multiple algorithms or parameter settings.
- [ ] Scale features where appropriate and fit preprocessing only on training data.
- [ ] Document score direction and threshold choice.
- [ ] Evaluate with labels when available; otherwise explain the review strategy.
- [ ] Include false-positive analysis, limitations, and monitoring plans.
