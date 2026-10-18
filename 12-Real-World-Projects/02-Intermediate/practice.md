# Practice Exercises — Intermediate Real-World Projects

Work through the exercises in sequence. Use the generated datasets in `examples.ipynb` where useful. Do not use real personal or confidential data without approval.

## Part A — Business framing and data audit

### 1. Write a business brief
For a churn model, write the decision to be supported, who will use the prediction, when it must be available, and what action may follow.

### 2. Define the target
Define “churn within the next 30 days.” List two ambiguities that must be resolved before creating labels.

### 3. Identify leakage
A table includes `cancellation_date`, `refund_after_cancel`, and `support_tickets_last_30d`. Which features could leak the outcome, and under what timing assumptions?

### 4. Audit a dataset
Write code that prints row/column counts, data types, missing-value counts, duplicate rows, and target distribution.

### 5. Data dictionary
Create a data dictionary with feature name, meaning, type, valid range/categories, source, and whether the feature is available at prediction time.

### 6. Sampling and representativeness
Explain how a dataset containing only customers who contacted support might bias churn predictions.

### 7. Train/test split
Explain why the split must happen before fitting an imputer, scaler, or encoder. Describe when a time-based split is preferable.

### 8. Missingness
Compare median imputation, most-frequent imputation, and dropping rows. State when each could be reasonable.

## Part B — EDA and feature engineering

### 9. Numeric distributions
Plot a histogram for tenure and monthly charges. Write two observations without claiming causation.

### 10. Categorical comparison
Compare churn rates by contract type. Include group sizes so a small group is not over-interpreted.

### 11. Correlation caution
Explain why a high correlation between support tickets and churn does not prove support tickets cause churn.

### 12. Outliers
Identify possible extreme values in a numeric column. Distinguish data errors from valid but unusual customers.

### 13. Feature engineering
Create `average_monthly_spend = total_spend / max(tenure_months, 1)`. Explain why you should inspect zero tenure and how this feature might leak information.

### 14. Class balance
Calculate churn prevalence. Explain why accuracy can be misleading when only a small percentage churn.

### 15. Segment stability
Describe how you would check whether a cluster or segment remains similar across different random seeds or time periods.

### 16. EDA summary
Write five bullet points for a business stakeholder: three findings, one caveat, and one next question.

## Part C — Modeling and evaluation

### 17. Baseline classifier
Build a `DummyClassifier` baseline and report its accuracy and recall for the positive class.

### 18. Logistic regression pipeline
Use `ColumnTransformer` with numeric imputation/scaling and categorical imputation/one-hot encoding, followed by logistic regression.

### 19. Confusion matrix
Given TP=42, FP=18, FN=28, TN=212, calculate precision, recall, and accuracy.

### 20. Threshold trade-off
Explain how increasing the classification threshold typically affects precision and recall. Give a scenario where higher recall is preferred.

### 21. ROC-AUC vs PR-AUC
Explain the difference and when precision-recall analysis is particularly informative.

### 22. Cross-validation
Describe how stratified cross-validation helps classification evaluation and why preprocessing must remain inside each fold's pipeline.

### 23. Regression baseline
For house prices, predict the training-set median for every test row. Explain why the baseline median must be computed from training data only.

### 24. Regression metrics
Explain MAE, RMSE, and R². Give an example where RMSE is more sensitive than MAE.

### 25. Error analysis
Group prediction errors by a relevant feature or target range. What might a large error gap between groups indicate?

### 26. Calibration
Explain what it means for predicted churn probabilities to be calibrated. Why does calibration matter when scores drive outreach?

### 27. Model comparison
Create a table comparing a baseline and at least two models. Include metrics, runtime/complexity, and one limitation per model.

### 28. Avoid overfitting the test set
Explain why repeatedly changing a model based on test-set results makes the final reported score optimistic.

## Part D — Unsupervised learning and business analysis

### 29. Prepare clustering features
Select three numeric customer behavior features. Explain why scaling matters for K-means.

### 30. Interpret clusters
For each cluster, report size, mean spend, tenure, and engagement. Give neutral labels based on observed values.

### 31. Choose cluster count
Compare inertia and silhouette score for several values of k. Explain why neither metric alone selects the business-optimal number of clusters.

### 32. Sales KPI definitions
Define revenue, net revenue, units sold, and gross margin. Explain how returns and discounts should be treated.

### 33. Time-aware validation
Explain why a random train/test split can be inappropriate for sales forecasting.

### 34. Campaign conversion
A campaign has 36 conversions from 600 recipients; another has 28 from 350. Calculate both conversion rates and explain why the higher rate alone does not establish a causal winner.

### 35. Simpson’s paradox
Describe how an aggregate campaign conversion rate can reverse when comparing within customer segments.

## Part E — Delivery, governance, and communication

### 36. Model card
Create a one-page model card with intended use, out-of-scope use, data, metrics, limitations, monitoring, and owner.

### 37. Privacy review
List five ways to reduce the privacy risk of a customer analytics project.

### 38. Reproducibility
Add a fixed random seed, dependency list, clear run instructions, and a record of dataset generation assumptions.

### 39. Executive summary
Write a 150-word summary containing the problem, method, key result, recommended action, uncertainty, and next step.

### 40. Capstone — churn intervention
Complete a churn project from synthetic data to recommendation. Include:
- business brief and target definition;
- data audit and EDA;
- baseline and pipeline model;
- holdout evaluation and error analysis;
- threshold decision tied to outreach capacity;
- limitations and responsible-use review;
- README and reproducible instructions.

**Deliverable:** a repository-ready project folder and a short presentation or report.

## Self-review

Score each from 0 (not yet) to 2 (can explain and demonstrate):

- [ ] Business question and target are precise.
- [ ] Data quality and leakage are addressed.
- [ ] EDA is supported by clear charts and caveats.
- [ ] Baseline and pipeline are reproducible.
- [ ] Metrics fit the decision.
- [ ] Errors and subgroup behavior are reviewed.
- [ ] Findings lead to a realistic recommendation.
- [ ] Privacy, fairness, limitations, and monitoring are discussed.

A high-quality submission explains not only what the model predicts, but also what decision it can safely support—and what it cannot establish.
