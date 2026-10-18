# Practice Exercises — End-to-End Data Science Projects

Complete the exercises in order. The capstone should combine the outputs into one reproducible project. Use synthetic or approved data only.

## Phase 1 — Problem framing and planning

### 1. Write a project charter
Define the business problem, stakeholder, decision, prediction time, target, time horizon, success metric, constraints, and out-of-scope uses.

### 2. Create acceptance criteria
Write at least five testable criteria for deciding whether the project is ready for a pilot. Include one model-quality criterion and one operational criterion.

### 3. Map the lifecycle
Create a diagram from data source to monitoring. Mark which steps are automated and which require human review.

### 4. Risk register
List at least six risks, such as leakage, missing data, privacy exposure, model drift, unclear ownership, and cost overruns. Give each a likelihood, impact, mitigation, and owner.

## Phase 2 — Data acquisition and validation

### 5. Data dictionary
Document feature name, type, meaning, valid values, source, sensitivity, and availability at prediction time.

### 6. Schema validation
Write a function that checks required columns and raises a clear error when a column is absent.

### 7. Quality report
Calculate missing-value rates, duplicate rows, numeric ranges, categorical cardinality, and target prevalence.

### 8. Data leakage review
For each feature, ask whether it is known at prediction time. Explain why a field recorded after the outcome cannot be used for a pre-outcome prediction.

### 9. Reproducible data split
Create a reproducible stratified train/test split for classification. Explain when a time-based or group-based split is more appropriate.

### 10. Data lineage
Write a short lineage record with source/generator, extraction or generation date, transformation steps, and version identifier.

## Phase 3 — EDA and feature engineering

### 11. EDA report
Create at least three charts: target distribution, one numeric distribution, and target rate by a category. Include group sizes where applicable.

### 12. Missingness analysis
Determine whether missingness may be systematic. Compare a simple imputation approach with dropping rows and explain the trade-offs.

### 13. Outlier policy
Define how you will identify likely data errors versus valid extreme observations. Document any treatment.

### 14. Feature engineering
Create one feature based on domain logic. Verify it uses only information available at the prediction time.

### 15. Reusable preprocessing
Build a scikit-learn `Pipeline` and `ColumnTransformer` for numeric and categorical columns.

## Phase 4 — Training and evaluation

### 16. Baseline
Create a baseline model and document why it is a useful point of comparison.

### 17. Candidate model
Train at least one candidate model using the preprocessing pipeline.

### 18. Evaluation report
For classification, include precision, recall, F1, ROC-AUC, and a confusion matrix. For regression, include MAE, RMSE, and residual analysis. Justify the metrics you select.

### 19. Cross-validation
Add appropriate cross-validation. Explain why stratified, group-aware, or time-aware validation may be needed for different data.

### 20. Threshold or decision policy
If using classification probabilities, choose a threshold using validation data and an explicit business constraint such as review capacity or false-negative cost.

### 21. Error analysis
Review at least two error slices, such as customer tenure bands or predicted-risk bands. Report sample sizes and avoid unsupported conclusions from small groups.

### 22. Test-set discipline
Write down how the final holdout will be used once. Explain what you would do if you have already repeatedly tuned against it.

## Phase 5 — Packaging and serving

### 23. Save a model pipeline
Serialize the fitted preprocessing/model pipeline with `joblib`. Record the model version and the library versions. Only load artifacts from trusted sources.

### 24. Reusable prediction function
Write a function that accepts a DataFrame, checks required features, and returns predictions or probabilities in a documented format.

### 25. API contract
Specify a request schema, response schema, validation errors, timeout behavior, and health endpoint for a prediction API.

### 26. Batch vs API
Compare batch scoring and online inference for your project. Choose one and justify latency, volume, freshness, and operational needs.

### 27. Container and configuration
Draft a Dockerfile and list configuration values that belong in environment variables. Explain why credentials must not be copied into the image.

### 28. Automated tests
Write tests for schema validation, valid prediction output shape, missing required columns, and a known edge case.

## Phase 6 — Operations and governance

### 29. Monitoring plan
Select metrics for service health, data quality, prediction distribution, and delayed model performance. Define ownership and an investigation action for each alert.

### 30. Drift response
Describe how you would detect input drift and how you would decide whether to investigate, retrain, or roll back.

### 31. Rollback plan
Specify a last-known-good application image and model artifact, rollback trigger, responsible owner, and post-rollback verification steps.

### 32. Privacy and security
List data minimization, access control, secret management, logging redaction, retention, and audit requirements.

### 33. Cost model
Estimate compute, storage, network, logging, and optional managed-service costs using a provider calculator or explicitly hypothetical assumptions.

### 34. Model card
Document intended use, out-of-scope use, data, metrics, limitations, subgroup behavior, monitoring, and model owner.

## Phase 7 — Communication and capstone

### 35. Technical README
Write setup, data acquisition/generation, training, evaluation, testing, inference, and troubleshooting instructions.

### 36. Executive summary
Write a 150–250 word summary for a non-technical stakeholder. Include the decision, evidence, limitations, recommendation, and next step.

### 37. Reproducibility audit
Ask another person to follow the README in a clean environment. Record missing steps and fix them.

### 38. Capstone — complete end-to-end project
Build one of the following:
- Customer churn decision system
- House-price estimation service
- Retail demand forecasting workflow
- Customer segmentation workflow
- Marketing performance analysis

Your repository must include problem framing, data documentation, validation, EDA, model/baseline, evaluation, tests, reusable inference, operational plan, model card, and reproduction instructions.

### 39. Pilot plan
Design a limited pilot with a human review process, success criteria, guardrails, monitoring, and a decision date. For predictive interventions, measure actual business outcomes rather than assuming model scores create value.

### 40. Final presentation
Create 5–8 slides: problem, data, method, evaluation, example outputs, limitations/risks, recommendation, and next steps.

## Self-assessment

Score each area from 0 (not complete) to 2 (complete and reproducible):

- [ ] Problem and target are clearly defined.
- [ ] Data source, schema, and quality are documented.
- [ ] Leakage and privacy risks are addressed.
- [ ] EDA and feature engineering are justified.
- [ ] Baseline and pipeline model are reproducible.
- [ ] Evaluation and error analysis support the decision.
- [ ] Prediction interface is tested.
- [ ] Monitoring, rollback, and cost are planned.
- [ ] README and model card are complete.
- [ ] A clean-environment reproduction succeeds.

A strong capstone is not merely a trained model. It is a complete, testable workflow with clear operational ownership and honest limitations.
