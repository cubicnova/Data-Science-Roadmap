# Practice: Model Monitoring

Use the notebook's synthetic reference and simulated production datasets, or substitute a dataset you are authorized to use.

## Exercise 1 — Input data quality

For both reference and current data, report:

- Number of rows and columns
- Missing-value rate by feature
- Minimum, maximum, mean, and standard deviation for numeric features
- Number of duplicate rows

Flag any unexpected schema mismatch before calculating drift statistics.

## Exercise 2 — Detect numeric data drift

For each numeric feature:

1. Calculate the reference and current means and standard deviations.
2. Calculate the standardized mean shift:

   `abs(current_mean - reference_mean) / max(reference_std, small_epsilon)`

3. Flag features whose shift exceeds a configurable threshold.
4. Explain why this score is only a simple diagnostic and not a formal universal drift test.

## Exercise 3 — Monitor prediction behavior

Run the same saved or in-memory model on reference and current data.

- Compare predicted class proportions.
- Compare positive-class probability distributions if the model supports `predict_proba`.
- Record sample counts and the time window represented by the current data.
- Explain why prediction drift does not prove performance degradation.

## Exercise 4 — Evaluate quality when labels arrive

Given current labels, calculate accuracy, precision, recall, and F1. Compare them with the baseline evaluation. If labels are not yet available, explicitly report quality as unavailable rather than inventing a metric.

## Exercise 5 — Create a monitoring report

Save a report containing:

- Timestamp and window name
- Reference/current sample counts
- Missing-value rates
- Feature drift indicators
- Prediction distribution
- Model-quality metrics, if labels exist
- Alert status and investigation notes

Use CSV or JSON. Do not include secrets or unnecessary personal information.

## Exercise 6 — Design alert rules

Write rules for at least three alert types:

- Service latency or error-rate breach
- Input schema or missing-value breach
- Feature drift or model-quality degradation

For each rule, specify a threshold, minimum sample size or duration, owner, and recommended investigation. Choose thresholds based on business requirements and baseline variability rather than treating example values as universal.

## Exercise 7 — Monitoring response plan

Write a short response plan for a sudden feature drift alert. Include validation of the incoming data, investigation of upstream changes, segment analysis, checking labels and business impact, and a decision about rollback or retraining.

## Reflection questions

1. How are data drift and concept drift different?
2. Why is monitoring latency not the same as monitoring model accuracy?
3. Why can a drift metric trigger a false alarm?
4. What is required to calculate reliable production performance metrics?
5. Why should drift detection not automatically trigger retraining in every system?
