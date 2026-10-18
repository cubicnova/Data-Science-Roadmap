# 01 — Ensemble Learning

Ensemble learning combines predictions from multiple models to improve generalization, stability, or predictive performance. This module introduces the main ensemble families using Python and scikit-learn.

## Learning objectives

By the end of this module, you should be able to:
- Explain why ensembles can outperform a single estimator.
- Distinguish bagging, boosting, voting, and stacking.
- Train and evaluate Random Forest, Extra Trees, AdaBoost, and Gradient Boosting models.
- Combine heterogeneous estimators with voting and stacking.
- Compare models using suitable metrics and cross-validation.
- Inspect feature importance without treating it as causal evidence.
- Avoid data leakage by fitting preprocessing and models only within training folds.

## Core concepts

| Method | Main idea | Typical strength | Watch out for |
|---|---|---|---|
| Bagging | Train estimators on bootstrap samples and aggregate | Reduces variance | More compute and less interpretability |
| Random Forest | Bagged decision trees plus random feature subsets | Strong tabular baseline | Feature importance can be biased or misleading |
| Extra Trees | Adds further randomness to tree splits | Often fast and robust | Performance is data-dependent |
| AdaBoost | Sequentially emphasizes examples previous learners miss | Can improve weak learners | Sensitive to noisy labels/outliers |
| Gradient Boosting | Sequentially fits learners to reduce a loss | Strong tabular performance | Needs tuning; can overfit |
| Voting | Aggregate different models' predictions | Benefits from complementary models | Soft voting needs probability estimates |
| Stacking | Train a meta-model on out-of-fold base-model predictions | Learns how to combine models | More complex; use CV to avoid leakage |

## Typical workflow

1. Define the task and a baseline.
2. Split data into training and held-out test sets.
3. Establish a baseline using a simple model.
4. Train candidate ensembles with fixed random seeds where supported.
5. Compare candidates with cross-validation and task-appropriate metrics.
6. Tune a small, justified parameter grid on training data only.
7. Evaluate the selected model once on the held-out test set.
8. Inspect errors, calibration, stability, feature importance, and operational cost.

## Environment setup

Python 3.10+ is recommended.

```bash
python -m venv .venv
# Windows: .venv\\Scripts\\activate
# macOS/Linux: source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install numpy pandas matplotlib scikit-learn jupyter
jupyter notebook
```

Open `examples.ipynb` in Jupyter or VS Code. Examples use scikit-learn's built-in datasets and generated data; no external dataset download is required.

## Files

- `README.md` — concepts, workflow, and setup.
- `examples.ipynb` — guided, runnable examples.
- `practice.md` — 40 progressive questions and coding exercises.
- `solution.md` — reference answers and explanations.

## Recommended study order

1. Understand bias, variance, and the baseline.
2. Compare a decision tree with bagging and Random Forest.
3. Explore boosting methods.
4. Combine models using voting and stacking.
5. Validate, tune, interpret, and document a mini-project.

## Good practices

- Keep the test set untouched during model selection.
- Use pipelines and cross-validation to prevent preprocessing leakage.
- Compare against a baseline; an ensemble is not automatically better.
- For imbalanced classification, report precision, recall, F1, PR-AUC, and a confusion matrix as appropriate—not accuracy alone.
- Use `random_state` for reproducible demonstrations, but validate findings across folds or seeds when needed.
- Fit ensembles within a realistic compute budget and consider inference latency.
- Treat feature importance as a model diagnostic, not a causal explanation.

## Further reading

- [scikit-learn: Ensemble methods](https://scikit-learn.org/stable/modules/ensemble.html)
- [scikit-learn: Model evaluation](https://scikit-learn.org/stable/modules/model_evaluation.html)
- [scikit-learn: Cross-validation](https://scikit-learn.org/stable/modules/cross_validation.html)
