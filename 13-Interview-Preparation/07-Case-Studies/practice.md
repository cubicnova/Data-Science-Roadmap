# Data Science Interview Case Studies — Practice

Use the framework in `README.md` for every prompt. Do not jump straight to an algorithm: clarify the business decision, define the unit of analysis, and select success metrics first.

## Case 1 — Customer churn

A subscription business says monthly churn has increased. It wants to predict customers likely to cancel and offer retention incentives.

1. What clarifying questions would you ask?
2. Define the target label, observation unit, and prediction horizon.
3. Which data sources and features would you request?
4. Identify at least five leakage or bias risks.
5. Propose a baseline and a validation strategy.
6. Explain when precision, recall, PR-AUC, ROC-AUC, and calibration would matter.
7. How would you choose an intervention threshold with limited campaign capacity?
8. How would you measure whether the retention campaign actually reduces churn?

## Case 2 — Fraud detection

A payment platform processes millions of transactions per day. Fraud is rare, and false positives frustrate legitimate customers.

1. Why is accuracy a weak primary metric?
2. How would you split the data and prevent leakage?
3. What metrics and operational constraints should be reported?
4. How would you handle delayed fraud labels and concept drift?
5. How would you design human review and escalation?
6. How would you estimate the cost of false negatives and false positives?

## Case 3 — Demand forecasting

A retailer needs weekly demand forecasts for thousands of products across locations.

1. Define the forecast horizon and forecast grain.
2. What simple baselines should be tested?
3. What is the difference between a random split and a rolling-origin evaluation?
4. How should promotions, holidays, stockouts, and new products be handled?
5. Compare MAE, RMSE, WAPE, and MASE. When can percentage metrics fail?
6. How would forecast uncertainty affect inventory decisions?

## Case 4 — A/B test analysis

A product team reports that a new onboarding flow increased activation by 4%.

1. What is the experimental unit and primary metric?
2. What checks should happen before comparing groups?
3. Explain randomization, sample-ratio mismatch, statistical significance, and practical significance.
4. Why is repeated peeking at p-values risky?
5. How would you handle multiple metrics and segments?
6. What guardrail metrics would you monitor?
7. What would you recommend if activation improves but paid conversion falls?

## Case 5 — Recommendation system

A media app wants to recommend content to improve engagement without creating repetitive feeds.

1. Define the objective and constraints.
2. What are useful popularity and personalized baselines?
3. How would you split interaction data to avoid future leakage?
4. Explain offline ranking metrics such as Precision@K, Recall@K, MAP, and NDCG.
5. Why may offline metrics disagree with online outcomes?
6. How would you address cold-start users/items, diversity, and popularity bias?
7. Design an online experiment and guardrails.

## Case 6 — Delivery-time prediction

A delivery platform wants to predict arrival time at checkout.

1. Define the target and when the prediction is made.
2. Which features are available at prediction time?
3. Identify leakage from post-checkout events.
4. Why might MAE and high-percentile absolute error both be useful?
5. How would you evaluate performance by geography, time of day, and weather?
6. How would the product use uncertainty rather than displaying false precision?

## Case 7 — Metric drop investigation

A dashboard shows that conversion fell 12% overnight, but traffic volume looks normal.

1. What do you verify before assuming user behavior changed?
2. How do you check event instrumentation, definitions, data freshness, and pipeline failures?
3. How would you segment the metric by device, region, browser, channel, and funnel step?
4. How do denominator changes and Simpson's paradox create misleading aggregate trends?
5. What evidence would make you roll back a release versus continue investigating?

## Case 8 — Model launch decision

A model improves offline AUC from 0.79 to 0.82, but the new model is slower and harder to explain.

1. What additional evidence is needed before launch?
2. How would you compare model quality, latency, cost, stability, and explainability?
3. Design a shadow deployment or canary rollout.
4. What monitoring and rollback triggers would you define?
5. How would you communicate the decision to a nontechnical stakeholder?

## Coding and analysis exercises

1. In `examples.ipynb`, change the random seed and compare metrics. Why can results change?
2. Add a feature that is a direct proxy for the target and explain how leakage can inflate metrics.
3. Compare the default 0.50 classification threshold with a lower threshold. Report precision and recall.
4. Build a segment-level error table by plan type or region.
5. Write a one-paragraph executive summary using only measured results.
6. Propose a time-based split for a scenario where customer behavior changes seasonally.

## A strong answer template

- **Goal:** What decision are we improving?
- **Clarifications and assumptions:** What is known, unknown, and constrained?
- **Data and target:** What is one row, what is the label, and when is it available?
- **Approach:** Baseline, features, validation, candidate methods.
- **Evaluation:** Metrics, slices, uncertainty, and error costs.
- **Action:** How output changes a decision and how impact is measured.
- **Risks:** Leakage, bias, privacy, drift, operational constraints.
- **Recommendation:** What to do next and what evidence would change the decision.
