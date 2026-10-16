# 02 — Machine Learning Workflow

A practical introduction to the end-to-end workflow used to turn a raw dataset into a reliable, reproducible machine learning solution.

## Learning objectives

By the end of this module, you should be able to:

- Describe the main stages of a machine learning project.
- Translate a business question into a machine learning task.
- Inspect data, identify the target, and distinguish features from labels.
- Split data into training, validation, and test sets without leakage.
- Build preprocessing and modeling pipelines with scikit-learn.
- Establish a baseline and select suitable evaluation metrics.
- Compare models fairly and document results.
- Recognize data leakage, overfitting, underfitting, and reproducibility risks.

## The ML workflow at a glance

1. **Define the problem** — identify the user, decision, target, constraints, and success metric.
2. **Acquire and understand data** — check source, permissions, schema, quality, and representativeness.
3. **Explore and validate** — inspect distributions, missing values, duplicates, target balance, and potential leakage.
4. **Prepare the data** — define features and target; split data before fitting learned preprocessing.
5. **Build a baseline** — use a simple, transparent model or a naive prediction rule.
6. **Train candidate models** — fit preprocessing and estimators using training data only.
7. **Validate and compare** — select metrics appropriate to the problem and compare models on the same split.
8. **Evaluate once on held-out test data** — estimate performance on unseen data after decisions are finalized.
9. **Communicate and deploy** — document limitations, monitor input and performance drift, and plan retraining.
10. **Reproduce and improve** — track data versions, code, parameters, metrics, and random seeds.

## Core concepts

### Problem framing
- **Classification:** predict a category, such as churn / no churn.
- **Regression:** predict a numeric value, such as delivery time.
- **Clustering:** group examples without a known target.
- **Business metric vs. model metric:** business outcomes guide success; model metrics help assess predictive quality.

### Data splitting
- Training data is used to fit model parameters.
- Validation data or cross-validation supports model selection and tuning.
- Test data is held back for a final, unbiased evaluation.
- For classification, `stratify=y` can preserve class proportions. For time-dependent data, split chronologically rather than randomly.

### Preprocessing and leakage
Imputation, scaling, encoding, feature selection, and other transformations that learn from data must be fitted on training data only. A scikit-learn `Pipeline` helps keep preprocessing inside cross-validation and prevents accidental leakage.

### Metrics
- Classification: accuracy, precision, recall, F1, ROC-AUC, confusion matrix.
- Regression: MAE, RMSE, and R².
- Choose metrics based on the cost of false positives, false negatives, and errors in the target's units. Do not choose a metric solely because it is popular.

### Reproducibility
Record the data source and version, split strategy, random seed, preprocessing, model, hyperparameters, metrics, software environment, and known limitations.

## Setup

Python 3.10+ is recommended.

```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate

python -m pip install --upgrade pip
python -m pip install numpy pandas matplotlib scikit-learn jupyter
jupyter notebook
```

Open `examples.ipynb` and run the cells from top to bottom. The notebook uses synthetic data generated locally; no external dataset or API is required.

## Suggested learning path

1. Read this overview and sketch the workflow in your own words.
2. Run `examples.ipynb` top to bottom.
3. Complete `practice.md` without looking at `solution.md`.
4. Compare your answers with the reference solutions and explain any differences.
5. Complete the mini-project and record the results in a short model card.

## Repository contents

- `README.md` — concepts, setup, and learning path.
- `examples.ipynb` — guided, runnable Python examples.
- `practice.md` — progressive exercises.
- `solution.md` — reference solutions with explanations.

## Troubleshooting

- **Module not found:** activate the intended virtual environment and install the listed packages.
- **Notebook kernel mismatch:** select the kernel associated with your `.venv`.
- **Results differ:** check package versions, random seeds, and split strategy. Small numeric differences can occur across versions.
- **Suspiciously high test score:** investigate leakage, duplicates across splits, target-derived features, and whether the split reflects real deployment.
- **Poor minority-class recall:** inspect the confusion matrix, class distribution, decision threshold, and business costs before changing the model.

## Good practice checklist

- [ ] Define the prediction target and the real-world decision.
- [ ] Confirm the data can be used for the intended purpose.
- [ ] Check schema, missingness, duplicates, and label quality.
- [ ] Decide on a split strategy before model tuning.
- [ ] Fit preprocessing only on training folds.
- [ ] Compare against a baseline.
- [ ] Use metrics suited to the task and costs.
- [ ] Keep the test set untouched until final evaluation.
- [ ] Record limitations and make the work reproducible.
