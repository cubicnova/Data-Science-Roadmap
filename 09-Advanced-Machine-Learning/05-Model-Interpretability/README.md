# Model Interpretability

Model interpretability helps us understand how a machine-learning model produces predictions, which features influence its outputs, and where its behavior may be unreliable. This module introduces global and local explanations, permutation importance, partial dependence, and SHAP-style reasoning, while highlighting common pitfalls.

## Learning Objectives

- Distinguish interpretable models from post-hoc explanation methods.
- Explain global versus local interpretability.
- Calculate and interpret permutation feature importance.
- Use partial dependence to explore average feature effects.
- Explain individual predictions with SHAP when the package is available.
- Recognize correlation, feature leakage, sampling, and explanation limitations.
- Communicate model behavior responsibly to technical and non-technical audiences.

## Contents

- [`examples.ipynb`](examples.ipynb) — guided, runnable notebook.
- [`practice.md`](practice.md) — exercises and mini-project.
- [`solution.md`](solution.md) — reference solutions.

## 1. What Is Model Interpretability?

Interpretability is the degree to which a person can understand a model's behavior. It can be useful for debugging, auditing, stakeholder communication, model governance, and identifying potential risks.

Two broad approaches:

- **Intrinsic interpretability:** use a model whose structure is relatively understandable, such as a small decision tree or linear model.
- **Post-hoc explanations:** analyze a fitted model using methods such as permutation importance, partial dependence, or SHAP.

No single method explains every aspect of a model. An explanation is an approximation or a particular view of model behavior, not necessarily a causal explanation.

## 2. Global vs. Local Explanations

| Type | Question | Example methods |
|---|---|---|
| Global | What patterns does the model use overall? | Permutation importance, partial dependence, global SHAP summaries |
| Local | Why did the model make this prediction for one row? | SHAP values, local surrogate explanations |
| Intrinsic | How does the model represent its decision process? | Linear coefficients, shallow decision trees |

Global explanations can hide subgroups or individual exceptions. Local explanations do not necessarily describe the model's overall behavior.

## 3. Permutation Feature Importance

Permutation importance measures how much a chosen evaluation score changes when one feature column is randomly shuffled. If shuffling a feature causes the score to fall substantially, the model relies on that feature for the chosen score and dataset.

Important caveats:
- Correlated features can substitute for one another, making each appear less important.
- Results depend on the chosen metric and evaluation sample.
- Importance is not evidence of causality.
- Calculate importance on held-out data when assessing generalization.

## 4. Partial Dependence

Partial dependence estimates the average model response as one or two selected features vary, averaging over the other observed features. It can reveal broad nonlinear patterns, but may create unrealistic combinations when features are correlated. Individual Conditional Expectation (ICE) curves show per-row response patterns and can reveal heterogeneity hidden by an average.

## 5. SHAP

SHAP (SHapley Additive exPlanations) uses a game-theoretic framework to assign feature contributions to a prediction relative to a baseline. A positive contribution generally moves the explained output upward and a negative contribution moves it downward; the precise meaning depends on the model output being explained.

SHAP values:
- are model- and explainer-dependent;
- can be affected by feature dependence assumptions;
- explain model output, not necessarily the real-world cause;
- may be expensive for some models and datasets.

The notebook's SHAP section is optional because the `shap` package may not be installed.

## 6. A Responsible Interpretation Workflow

1. Define the audience and the question the explanation should answer.
2. Establish a baseline and evaluate the model on held-out data.
3. Use multiple complementary explanation methods where appropriate.
4. Check feature correlations, data leakage, missingness, and subgroup behavior.
5. Verify explanations are stable across samples or model runs.
6. Report limitations and avoid causal language unless supported by a suitable causal design.
7. Treat explanations as one input to model review, not as proof that a model is fair or safe.

## Setup

```bash
python -m pip install numpy pandas matplotlib scikit-learn jupyter
# Optional, for SHAP examples:
python -m pip install shap
jupyter notebook examples.ipynb
```

## References

- [scikit-learn: Inspection and model interpretation](https://scikit-learn.org/stable/inspection.html)
- [scikit-learn: Permutation feature importance](https://scikit-learn.org/stable/modules/permutation_importance.html)
- [SHAP documentation](https://shap.readthedocs.io/)
