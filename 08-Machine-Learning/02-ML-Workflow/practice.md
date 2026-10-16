# Practice — Machine Learning Workflow

Work through the exercises in order. Use Python, pandas, and scikit-learn where appropriate. Do not consult `solution.md` until you have attempted each question.

## Beginner: workflow and framing

1. List the major stages of an ML workflow in the order you would normally perform them.
2. A delivery company wants to estimate delivery duration in minutes. Is this classification or regression? Name a suitable metric.
3. A bank wants to flag transactions for manual review. Give one possible target and explain why accuracy alone may be misleading.
4. Explain the difference between a feature and a target using a student-score example.
5. Give two questions you would ask a stakeholder before building a churn model.
6. Explain why data permissions, provenance, and representativeness matter before modeling.
7. What is a baseline model? Why should a complex model be compared with one?
8. Match each split to its purpose: training, validation, test.
9. Explain overfitting in your own words and give one warning sign.
10. Name four checks you would perform immediately after loading a dataset.

## Intermediate: data preparation and evaluation

11. Create a pandas DataFrame with `age`, `income`, and `churn` columns. Identify `X` and `y`.
12. Split a binary classification dataset into 80% train and 20% test using `train_test_split`, `random_state=42`, and stratification.
13. Why should a scaler be fitted only on training data? Describe the leakage risk.
14. Build a preprocessing pipeline that imputes missing numeric values with the median and standardizes numeric features.
15. Extend the pipeline to fit a logistic regression classifier.
16. For a dataset with 95% negative and 5% positive cases, explain why a model predicting only the negative class can have high accuracy but low usefulness.
17. Define precision and recall in the context of fraud detection.
18. A regression model has MAE = 4.2. Interpret this value in plain language, assuming the target is measured in minutes.
19. When might a chronological split be preferable to a random split?
20. Explain why the test set should not be used repeatedly to choose model settings.
21. Describe two ways to address missing values and one risk of each.
22. A feature is calculated using information that only becomes available after the outcome. What is the issue, and how would you handle it?
23. Create a confusion matrix and calculate precision, recall, and F1 from `TP=40`, `FP=10`, and `FN=20`.
24. Why can cross-validation provide a more stable model-selection estimate than one validation split?

## Advanced: model selection and reliability

25. Use `make_classification` to generate a dataset and compare a `DummyClassifier` baseline with logistic regression.
26. Use a `Pipeline` and `ColumnTransformer` to preprocess numeric and categorical features without leakage.
27. Compare accuracy, precision, recall, F1, and ROC-AUC. State when you would prioritize each.
28. Explain the difference between data leakage and overfitting. Give an example of each.
29. Propose a reproducibility checklist for a teammate who must rerun your experiment six months later.
30. A model performs well offline but poorly after deployment. List at least five plausible causes and what you would inspect.
31. Explain why threshold selection is a decision problem rather than simply a model-training problem.
32. Design an experiment to compare two candidate models fairly. Include splits, metrics, and what must remain fixed.
33. Explain why feature importance does not automatically imply causation.
34. Design a monitoring plan for a deployed churn model, including input data, prediction quality, and business outcomes.

## Mini-project: end-to-end workflow

35. Use a synthetic or approved tabular dataset to predict a binary outcome.
    - Write a one-paragraph problem statement and identify the intended decision.
    - Identify target, features, and any fields that must be excluded.
    - Inspect missing values, duplicates, class balance, and suspicious columns.
    - Create a reproducible train/test split.
    - Establish a `DummyClassifier` baseline.
    - Build a leakage-safe preprocessing and logistic-regression pipeline.
    - Report accuracy, precision, recall, F1, ROC-AUC, and a confusion matrix where appropriate.
    - Compare the baseline and candidate model.
    - Write three limitations and two monitoring signals.
36. Create a one-page model card with: intended use, out-of-scope use, dataset and split, metrics, limitations, fairness considerations, and monitoring plan.

## Mini-project rubric (100 points)

| Area | Points | Evidence |
|---|---:|---|
| Problem framing | 10 | Clear target, decision, and success criterion |
| Data inspection | 15 | Quality checks and leakage investigation |
| Split strategy | 15 | Reproducible and appropriate split |
| Baseline | 10 | Sensible naive benchmark |
| Pipeline | 20 | Preprocessing fitted within training workflow |
| Evaluation | 15 | Appropriate metrics and interpretation |
| Reproducibility | 5 | Seed, versions, and steps recorded |
| Communication | 10 | Limitations and monitoring documented |

## Reflection

After finishing, answer:
- Which workflow step was easiest to overlook?
- Which decision could cause leakage?
- What would change if the data were time-dependent?
- What evidence would you need before deploying the model?
