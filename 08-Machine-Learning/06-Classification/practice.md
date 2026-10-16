# Practice - Classification

Attempt the questions before checking `solution.md`. Use Python and Scikit-learn for coding exercises.

## Part A - Concepts (1–10)

1. Define classification and explain how it differs from regression.
2. Explain binary, multiclass, and multilabel classification with an example of each.
3. What are features and target labels?
4. Why should a classification dataset be split into training and test sets?
5. What does stratification do during a train/test split?
6. Explain true positive, true negative, false positive, and false negative.
7. Define precision, recall, F1-score, and accuracy.
8. Why can accuracy be misleading on an imbalanced dataset?
9. Explain how changing a probability threshold can affect precision and recall.
10. Why should model selection and threshold tuning happen without repeatedly using the final test set?

## Part B - Choose an Algorithm or Metric (11–16)

11. You need a simple interpretable baseline for a binary classification task. Name one suitable algorithm.
12. Your features have very different scales and you want to use a distance-based algorithm. Name the algorithm and a preprocessing step to consider.
13. A fraud detection dataset contains 1% positive cases. Which metrics would you report beyond accuracy, and why?
14. A medical screening system must minimize missed positive cases. Which metric is especially important, and what trade-off should be monitored?
15. A spam filter should avoid marking legitimate emails as spam. Which metric is especially relevant?
16. Give two reasons why a model with high training accuracy may perform poorly on unseen data.

## Part C - Iris Multiclass Classification (17–23)

Use `sklearn.datasets.load_iris`.

17. Load the Iris dataset and print its feature names, target names, and shape.
18. Split the data into 80% training and 20% test data with `random_state=42` and `stratify=y`.
19. Train a `LogisticRegression(max_iter=1000)` model.
20. Report test accuracy, confusion matrix, and classification report.
21. Train a `DecisionTreeClassifier` with a fixed `random_state`. Compare its test results with Logistic Regression.
22. Train a `DummyClassifier(strategy="most_frequent")` and compare it with the learned models.
23. Explain why one train/test split is not enough to prove that a model will generalize across every possible sample.

## Part D - Binary Classification and Metrics (24–30)

Use `sklearn.datasets.make_classification` to generate a reproducible binary dataset.

24. Generate 1,000 samples with 20 features and a fixed random state.
25. Split into training and test sets with stratification.
26. Train a `RandomForestClassifier` and predict the test labels.
27. Calculate accuracy, precision, recall, F1-score, and a confusion matrix.
28. If the model supports `predict_proba`, extract positive-class probabilities and compute ROC-AUC.
29. Use `precision_recall_curve` to inspect precision and recall at different thresholds.
30. Describe a practical method for choosing a threshold without using the final test set to tune it.

## Part E - Model Comparison and Imbalance (31–35)

31. Compare Logistic Regression, Decision Tree, and Random Forest on the same split.
32. Create a metric table with accuracy, precision, recall, and F1-score for each model.
33. Change `class_weight` where supported and compare the impact on recall and precision.
34. Explain why resampling should be performed only inside the training workflow.
35. Describe when ROC-AUC and precision-recall analysis provide complementary information.

## Part F - Mini-Projects (36–40)

36. **Iris classifier:** train and compare two classifiers, show a confusion matrix, and write a conclusion.
37. **Spam detection plan:** describe a text classification pipeline, suitable features, evaluation metrics, and likely errors.
38. **Fraud detection:** design an evaluation plan for highly imbalanced data. Explain the relative cost of false positives and false negatives.
39. **Threshold analysis:** on validation data, compare at least three thresholds and report precision/recall. Do not tune the threshold using the final test set.
40. **Classification report:** choose a real-world classification problem and submit the goal, dataset description, split strategy, baseline, model, metrics, error analysis, and limitations.

## Submission Checklist

- [ ] Use a reproducible split.
- [ ] Compare against a baseline.
- [ ] Use metrics appropriate to the class balance and error costs.
- [ ] Inspect confusion matrix and class-wise results.
- [ ] Avoid preprocessing or resampling leakage.
- [ ] Explain limitations rather than reporting a score alone.
