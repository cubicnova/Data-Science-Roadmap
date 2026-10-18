# Data Science Interview Case Studies — Suggested Solutions

These are example answer structures, not scripts to memorize. Strong interviews make assumptions explicit and adapt the analysis to the business decision.

## Case 1 — Customer churn

### Clarify the goal
Ask how churn is defined (cancellation, non-renewal, or inactivity), what time window matters, when interventions can occur, how many customers can be contacted, and what an incentive costs. Separate prediction from the causal question of whether an offer prevents churn.

### Define the target
One row per eligible customer at a defined snapshot date. For example, label whether the customer cancels within the next 30 days. Features must be available at the snapshot time. Use chronological training and test windows if the model will predict future periods.

### Data and leakage
Potential sources include subscription tenure, plan, product usage, support contacts, payment history, billing changes, and prior campaign exposure. Leakage risks include cancellation-request status recorded after the snapshot, post-cancellation support codes, future usage, target-derived fields, and features computed using the entire dataset.

### Baseline and evaluation
Start with a simple rule such as recent engagement decline or a logistic-regression pipeline. Report PR-AUC for an imbalanced target, ROC-AUC for ranking, precision/recall at the operating threshold, calibration, and lift in the top campaign-capacity segment. Compare against the baseline using the same held-out period.

### Threshold and campaign measurement
Choose the threshold using capacity and expected net value, not a default of 0.50. For a customer, expected value depends on churn risk, the offer's incremental retention effect, customer value, and offer cost. Run a randomized holdout among eligible customers to measure incremental retention; a high-risk model alone does not prove an offer works.

## Case 2 — Fraud detection

- Accuracy can be high by predicting every transaction as legitimate when fraud prevalence is tiny.
- Use time-aware splits, avoid features created after a transaction is reviewed, and prevent the same entity from leaking across splits when appropriate.
- Report precision, recall, PR-AUC, false-positive rate, value-weighted loss, review volume, latency, and customer friction.
- Labels can arrive late; treat recent data as partially labeled and avoid declaring recent predictions correct too early.
- Use risk tiers: block only when evidence and costs justify it, challenge uncertain transactions, and route ambiguous cases to review.
- Re-estimate thresholds as fraud tactics, customer mix, and review capacity change.

## Case 3 — Demand forecasting

- Define the grain (product-location-week), forecast horizon, and when forecasts are issued.
- Baselines: last period, seasonal naive, moving average, and a simple global or per-series model.
- Rolling-origin evaluation trains only on the past and evaluates later periods; random splits can leak future patterns.
- Promotions and holidays must be known at forecast time. Stockouts censor observed demand; new products need metadata or category-level priors.
- MAE is interpretable in units; RMSE penalizes large errors more; WAPE scales total absolute error by total actual volume but can hide low-volume series; MASE compares errors with a naive baseline. MAPE is undefined or unstable when actuals are zero or near zero.
- Prediction intervals and quantiles can support service-level and inventory decisions; forecast uncertainty should be evaluated, not assumed.

## Case 4 — A/B test

- Define a primary metric, experimental unit, eligibility rules, and analysis window before examining results.
- Check assignment balance, sample-ratio mismatch, instrumentation, exposure, duplicate users, and metric consistency.
- Randomization helps balance confounders in expectation. Statistical significance is not the same as business importance.
- Repeated unplanned peeking inflates false-positive risk; use a pre-specified fixed horizon or a valid sequential-testing method.
- Predefine primary and guardrail metrics; account for multiple comparisons when many outcomes are tested.
- If activation improves but paid conversion declines, quantify both effects and uncertainty, inspect the funnel, and decide based on pre-agreed business priorities rather than cherry-picking one metric.

## Case 5 — Recommendation system

- Define the target: clicks, watch time, satisfaction, retention, or a constrained combination. Add guardrails for diversity, freshness, safety, and user control.
- Start with popularity, recency, and item-to-item baselines.
- Use temporal splits so interactions from the future cannot train recommendations evaluated in the past.
- Precision@K and Recall@K evaluate relevant items among the top K; MAP rewards ranking quality across relevant items; NDCG discounts relevant items lower in the list.
- Offline logs reflect the old recommendation policy and exposure bias; online tests reveal behavioral impact but need guardrails.
- Use content or metadata for cold-start, diversify candidate reranking, and monitor exposure concentration.
- A/B test with engagement as a primary metric and guardrails such as hides, skips, complaints, and long-term retention.

## Case 6 — Delivery-time prediction

- Predict ETA at checkout using only information available at that time. Define whether the target is order placed to delivery, courier pickup to delivery, or another operational interval.
- Candidate features include distance, region, time, weather known at prediction time, historical traffic, merchant preparation estimates, and current capacity.
- Actual courier route, completed preparation time, and post-checkout delays can leak future information if not available at prediction time.
- MAE describes typical absolute error; high-percentile error reveals poor tail cases. Evaluate calibration of prediction intervals if shown to customers.
- Slice performance by geography, peak periods, weather, and order type; inspect coverage and sample sizes.
- Present rounded ranges when uncertainty is high and update estimates as new information becomes available.

## Case 7 — Metric drop investigation

1. Verify freshness, event volume, schema changes, logging releases, time zones, and dashboard definitions.
2. Compare raw event counts with downstream tables; inspect late-arriving data, failed jobs, deduplication, and denominator logic.
3. Segment by device, region, browser, acquisition channel, app version, and funnel step.
4. Aggregate conversion can move because the traffic mix changes even when each segment is stable; compare within-segment rates and weighted contributions.
5. Roll back when there is credible evidence of a release-linked severe regression and rollback is safe. Otherwise isolate the issue, quantify uncertainty, and keep stakeholders informed with a clear next update time.

## Case 8 — Model launch decision

AUC alone is not enough. Compare operating-threshold precision/recall, calibration, subgroup performance, stability over time, latency at expected load, infrastructure cost, explainability needs, and business impact.

A safe rollout:
1. Run offline validation and data/leakage checks.
2. Deploy in shadow mode and compare predictions, latency, and failure rates without affecting decisions.
3. Canary a small eligible share with pre-defined guardrails.
4. Expand only if business and technical criteria hold.
5. Keep a rollback path to the previous model or rules-based baseline.

Monitoring should include input quality, feature drift, score distribution, outcome metrics once labels mature, subgroup errors, latency, errors, and resource usage. Set owners and response actions for every alert.

## Coding exercise — threshold comparison

```python
from sklearn.metrics import precision_score, recall_score, f1_score

# y_test: true binary labels; probabilities: predicted probability for positive class
for threshold in [0.30, 0.50, 0.70]:
    predictions = (probabilities >= threshold).astype(int)
    print({
        "threshold": threshold,
        "precision": precision_score(y_test, predictions, zero_division=0),
        "recall": recall_score(y_test, predictions, zero_division=0),
        "f1": f1_score(y_test, predictions, zero_division=0),
    })
```

Threshold choice should be based on business costs and capacity. Do not tune the threshold on the final test set; choose it using validation data and report final performance once on untouched test data.

## Example executive summary

> We built a baseline and a logistic-regression model to rank customers by 30-day churn risk on synthetic data. We evaluated them on a held-out stratified test set and compared ranking metrics and precision/recall at multiple thresholds. The selected operating point should be chosen using campaign capacity and expected incremental value, then validated with a randomized retention-offer holdout. Because the data are synthetic, the results demonstrate a workflow rather than predict real business outcomes.

Replace this example with the notebook's actual measured results before presenting it in an interview.
