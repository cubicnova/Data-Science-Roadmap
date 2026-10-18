# Practice — Imbalanced Datasets

Complete the exercises in sequence. Use Python, pandas, scikit-learn, and imbalanced-learn. Keep the final test set untouched during model and threshold selection.

**Suggested environment:** see `README.md`. Fix random seeds for reproducible demonstrations and compare methods on identical cross-validation folds.


## Foundations

1. Define class imbalance and give two real-world examples where the positive class is rare.
2. Explain why a classifier that predicts only the majority class may still report high accuracy.
3. Define true positive, true negative, false positive, and false negative for a fraud-detection scenario.
4. Compare precision and recall. Give an example where recall is more important and one where precision is more important.
5. What does F1 measure, and what important information does it omit?
6. Compare ROC-AUC and average precision. Why can a precision-recall view be more informative for rare positives?

## Data inspection

7. Create a DataFrame with 1,000 samples and a 95:5 class distribution. Print class counts and prevalence.
8. Use `make_classification` to generate a binary dataset with 2% minority-class prevalence. Plot the class distribution.

## Splitting

9. Create stratified train/test partitions and verify that the minority class appears in both sets.

## Baseline

10. Build a `DummyClassifier` baseline and report accuracy, balanced accuracy, precision, recall, F1, and average precision.
11. Train logistic regression without imbalance handling. Inspect its confusion matrix and classification report.

## Metrics

12. Calculate precision, recall, specificity, F1, and balanced accuracy from a confusion matrix by hand or in Python.
13. Plot a precision-recall curve and explain how the operating threshold changes precision and recall.

## Class weights

14. Train logistic regression with `class_weight='balanced'`. Compare its minority-class recall and precision with the baseline.
15. Compare class weights with explicit weights such as `{0: 1, 1: 5}`. Explain why the weights must be selected using validation data.

## Oversampling

16. Apply `RandomOverSampler` to training data only. Compare class counts before and after resampling.

## Undersampling

17. Apply `RandomUnderSampler` to training data only. Discuss the trade-off between training speed and information loss.

## SMOTE

18. Use `SMOTE` inside an imbalanced-learn pipeline with scaling and logistic regression. Compare cross-validation average precision.

## Leakage

19. Explain why applying SMOTE before train/test split causes leakage. Describe how an imbalanced-learn pipeline prevents this.

## Cross-validation

20. Use `StratifiedKFold` to compare unweighted logistic regression, class weighting, oversampling, and SMOTE on the same folds.

## Model comparison

21. Compare logistic regression, a decision tree, and Random Forest on the imbalanced dataset using average precision and balanced accuracy.

## Thresholds

22. Choose a probability threshold to achieve a target recall on validation data, then report precision and recall at that threshold.

## Threshold costs

23. Define a cost matrix for false positives and false negatives. Select a validation threshold that minimizes expected cost.

## Calibration

24. Explain why class weighting and resampling can change the meaning of predicted probabilities. Describe how to assess calibration.

## Metrics

25. Explain why ROC-AUC may appear strong while precision is poor when positive prevalence is very low.

## Confusion matrix

26. Plot and interpret a confusion matrix. Translate false-positive and false-negative counts into operational impact.

## Pipelines

27. Build a pipeline that scales numeric features and applies SMOTE before fitting a classifier. Ensure each operation is trained within CV.

## Hyperparameter tuning

28. Tune a class-weighted model with cross-validation using average precision as the scoring metric.

## Feature scaling

29. Explain why scaling matters for SMOTE and distance-based algorithms, but usually matters less for tree-based models.

## Robustness

30. Repeat evaluation over multiple stratified folds and report mean and standard deviation for average precision and recall.

## Data drift

31. Describe how changes in positive prevalence can affect precision, even if recall and specificity remain unchanged.

## Deployment

32. Propose an alert threshold and monitoring plan for an imbalanced fraud or intrusion-detection system.

## Fairness

33. Explain why aggregate metrics may hide different false-positive and false-negative rates across subgroups.

## Error analysis

34. Review false positives and false negatives and propose three ways to improve the data or decision process.

## Experiment design

35. Create a comparison table recording dataset split, resampling strategy, model, metrics, and runtime.

## Mini-project

36. Build a leakage-safe end-to-end classification workflow comparing baseline, class weights, random oversampling, and SMOTE.
37. Create a threshold-selection report using validation data, a stated cost function, and final test metrics.

## Review

38. Write a model-selection memo explaining why your chosen metric and imbalance strategy match the business problem.
39. List five common mistakes when handling imbalanced data and explain how you would prevent each.

## Challenge

40. Use a realistic or synthetic dataset with less than 1% positives. Compare AP against the positive prevalence and justify whether the model adds value.

## Submission checklist

- [ ] Positive class and operational costs are explicitly defined.
- [ ] Class distribution and prevalence are reported.
- [ ] A simple baseline is included.
- [ ] Metrics include precision, recall, and a prevalence-aware metric such as average precision.
- [ ] Splitting occurs before resampling.
- [ ] Resampling is performed only inside training folds.
- [ ] Thresholds and hyperparameters are selected without using the test set.
- [ ] Confusion matrix and limitations are discussed.
