# 04 — Imbalanced Datasets

Real-world classification datasets often contain far fewer examples of one class than another. With imbalanced data, a model can achieve high accuracy while missing most minority-class cases. This module covers reliable evaluation, resampling, class weighting, threshold selection, and leakage-safe model comparison.

## Learning objectives

By the end of this module, you should be able to:
- Explain why class imbalance can make accuracy misleading.
- Establish a baseline and inspect class distributions.
- Evaluate classifiers with precision, recall, F1, balanced accuracy, ROC-AUC, and average precision.
- Use stratified splits and cross-validation.
- Compare class weights, random oversampling, random undersampling, and SMOTE.
- Avoid resampling before train/test splitting.
- Select a decision threshold using validation data and explicit error costs.
- Explain the limits of resampling and the importance of calibration.

## Key concepts

| Technique / metric | What it does | Important caution |
|---|---|---|
| Class weights | Penalizes errors on selected classes more heavily | May trade precision for recall; tune on validation data |
| Random oversampling | Duplicates minority-class training examples | Can overfit repeated examples |
| Random undersampling | Removes majority-class examples | May discard useful information |
| SMOTE | Synthesizes minority examples by interpolating neighbors | Can create unrealistic samples; scale features and apply only within training folds |
| Precision | Fraction of predicted positives that are true positives | Can fall as recall is increased |
| Recall / sensitivity | Fraction of actual positives found | Important when missed positives are costly |
| F1 | Harmonic mean of precision and recall | Does not include true negatives and assumes equal importance of precision/recall |
| Balanced accuracy | Average recall across classes | Useful when class counts differ |
| ROC-AUC | Ranking quality across ROC thresholds | Can look optimistic with severe imbalance |
| Average precision (AP) | Summarizes the precision-recall curve | Often informative for rare positive classes; compare against prevalence |

## Recommended workflow

1. Define which class is the operationally important positive class.
2. Inspect class counts and the positive-class prevalence.
3. Create stratified train, validation (if needed), and test partitions.
4. Build a dummy or simple baseline.
5. Choose metrics based on the cost of false positives and false negatives.
6. Compare an unmodified model, class weighting, and resampling inside cross-validation.
7. Tune model parameters and thresholds using training/validation data only.
8. Evaluate the selected workflow once on the untouched test set.
9. Inspect the confusion matrix, probability calibration, subgroup performance, and operational constraints.

## Environment setup

Python 3.10+ is recommended.

```bash
python -m venv .venv
# Windows: .venv\\Scripts\\activate
# macOS/Linux: source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install numpy pandas matplotlib scikit-learn imbalanced-learn jupyter
jupyter notebook
```

Open `examples.ipynb` in Jupyter or VS Code. The examples use generated data and built-in scikit-learn datasets; no external download is required.

## Files

- `README.md` — concepts, evaluation guidance, and setup.
- `examples.ipynb` — runnable walkthrough with generated imbalanced data.
- `practice.md` — progressive theory, coding, and project exercises.
- `solution.md` — reference answers and working Python examples.

## Good practices

- Split before resampling. Never resample the full dataset before splitting.
- Put resampling steps inside an `imblearn.pipeline.Pipeline` so each CV fold resamples only its training partition.
- Do not use the test set to choose thresholds, metrics, or hyperparameters.
- Report the confusion matrix and per-class precision/recall; do not report accuracy alone.
- The default 0.5 threshold is not universally optimal.
- Compare models at the same folds and with the same metrics.
- Synthetic samples are not new independent real-world observations.
- Use domain expertise to define costs, acceptable alert volume, and the operational decision threshold.

## Further reading

- [scikit-learn: Classification metrics](https://scikit-learn.org/stable/modules/model_evaluation.html#classification-metrics)
- [scikit-learn: Class-weighted models](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)
- [imbalanced-learn: User guide](https://imbalanced-learn.org/stable/user_guide.html)
- [imbalanced-learn: Common pitfalls](https://imbalanced-learn.org/stable/common_pitfalls.html)
