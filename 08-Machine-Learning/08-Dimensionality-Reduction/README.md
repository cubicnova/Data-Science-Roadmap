# 08 — Dimensionality Reduction

Dimensionality reduction transforms data with many features into a smaller representation while attempting to preserve useful information. It can support visualization, reduce redundancy, improve computational efficiency, and sometimes help downstream models generalize. It can also discard information, so validate its effect for the intended task.

## Learning objectives
- Explain the curse of dimensionality and why feature count matters.
- Distinguish feature selection from feature extraction.
- Apply Principal Component Analysis (PCA) and interpret explained variance.
- Use t-SNE and UMAP-style embeddings for exploratory visualization.
- Understand TruncatedSVD for sparse data and random projection.
- Scale and impute data correctly before dimensionality reduction.
- Prevent leakage by fitting transformations only on training data.
- Evaluate whether a reduced representation preserves information relevant to the task.
- Communicate the limitations of low-dimensional plots.

## Main techniques

### Principal Component Analysis (PCA)
PCA is a linear transformation that creates orthogonal components ordered by the variance they explain. It is useful for compression, noise reduction, and preprocessing. Standardize features when their units/scales differ and equal scale is meaningful; scaling is not universally correct if raw variance carries intended meaning. PCA components are combinations of original features, so interpretation may be less direct.

### Explained variance
`explained_variance_ratio_` reports the fraction of variance captured by each component. Cumulative explained variance helps compare component counts, but a threshold such as 95% is a heuristic, not a guarantee that 95% of predictive information is retained.

### t-SNE
t-SNE is primarily a visualization method for local neighborhood structure in two or three dimensions. Its layout can vary with parameters and random initialization. Distances between well-separated islands and apparent cluster sizes should not be interpreted as faithful global geometry. For large feature spaces, PCA or TruncatedSVD preprocessing can reduce computation.

### UMAP
UMAP is a nonlinear manifold-learning technique often used for visualization and general-purpose embeddings. It can preserve local neighborhood structure and may support transforming new observations, depending on the fitted implementation. Install separately with `pip install umap-learn`; the core notebook does not require it.

### TruncatedSVD and random projection
- `TruncatedSVD` is useful for sparse matrices such as TF-IDF text features because it does not center the data.
- Random projection provides a computationally efficient approximate embedding with distance-preservation guarantees under suitable conditions.
- Feature agglomeration combines similar features into groups.
- Feature selection keeps a subset of the original features rather than creating new component axes.

## Comparison

| Method | Linear? | Common use | Key caution |
|---|---|---|---|
| PCA | Yes | Compression, preprocessing, 2D/3D projection | Captures variance, not necessarily target relevance |
| t-SNE | No | Exploratory 2D/3D visualization | Primarily local structure; global distances can mislead |
| UMAP | No | Visualization and embeddings | Results depend on parameters; validate stability |
| TruncatedSVD | Yes | Sparse matrices and text features | Components may be harder to interpret |
| Random projection | Generally linear map | Fast dimensionality reduction | Approximate representation; evaluate downstream impact |

## Workflow
1. Define the purpose: visualization, storage, speed, denoising, or downstream prediction.
2. Inspect feature scales, missingness, correlations, sparsity, and sample count.
3. Split train/test data before fitting learned transformations for predictive tasks.
4. Impute and scale where justified.
5. Fit the reducer on training data and transform held-out data.
6. Inspect explained variance or embedding structure as appropriate.
7. Compare downstream performance with and without reduction.
8. Record parameters, random seeds, software versions, and limitations.

## Setup

Python 3.10+ recommended.

```bash
python -m venv .venv
# Windows:
.venv\\Scripts\\activate
# macOS/Linux:
source .venv/bin/activate

python -m pip install --upgrade pip
python -m pip install numpy pandas matplotlib scikit-learn jupyter
jupyter notebook
```

Optional UMAP support: `python -m pip install umap-learn`

Run `examples.ipynb` from top to bottom. Core examples use locally generated synthetic data and do not require UMAP.

## Repository contents
- `README.md` — concepts and workflow.
- `examples.ipynb` — runnable PCA, t-SNE, TruncatedSVD, and random-projection examples.
- `practice.md` — exercises and mini-project.
- `solution.md` — reference answers and Python examples.

## Good-practice checklist
- [ ] Define the reason for reducing dimensions.
- [ ] Fit imputation, scaling, and reduction on training data only.
- [ ] Choose scaling based on feature semantics.
- [ ] Interpret explained variance as variance, not predictive value.
- [ ] Treat t-SNE/UMAP plots as exploratory visualizations.
- [ ] Compare downstream performance with the original feature space.
- [ ] Check stability across seeds and parameters.
- [ ] Record component count, parameters, seed, and limitations.

## References
- [scikit-learn: Unsupervised dimensionality reduction](https://scikit-learn.org/stable/modules/unsupervised_reduction.html)
- [scikit-learn: PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html)
- [scikit-learn: t-SNE](https://scikit-learn.org/stable/modules/generated/sklearn.manifold.TSNE.html)
- [UMAP documentation](https://umap-learn.readthedocs.io/en/latest/)
