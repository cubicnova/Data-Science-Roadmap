# Reference Solutions — Dimensionality Reduction

These are reference approaches. Results vary with data, scaling, random seed, and library versions.

## Beginner answers (1–12)
1. Dimensionality reduction maps data to fewer dimensions. Reasons include visualization, lower compute/storage costs, noise reduction, and reduced redundancy.
2. Feature selection retains some original columns; feature extraction creates new representations, such as PCA components.
3. In high-dimensional spaces, data can become sparse and distance/similarity intuitions can degrade; more data may be needed to estimate patterns.
4. PCA finds orthogonal directions capturing as much variance as possible, then projects observations onto a selected number of directions.
5. A principal component is a linear combination of original features defined by a direction in feature space.
6. Each ratio gives the share of total variance explained by one component; cumulative variance sums those shares across components.
7. If units differ, high-scale features can dominate variance. Standardization makes scales comparable when that is appropriate.
8. Scaling changes the relative influence of features. If absolute variance is meaningful by design, standardizing everything may discard useful weighting.
9. PCA is linear and variance-oriented; t-SNE is nonlinear and primarily designed to visualize local neighborhood structure.
10. t-SNE can distort global distances and cluster sizes; separated islands do not necessarily indicate strongly separated groups in the original space.
11. UMAP is used for nonlinear visualization and embeddings; outputs depend on data and settings.
12. TF-IDF matrices are sparse. TruncatedSVD does not center the matrix, avoiding the densification that centering can cause.

## PCA examples (13–18)

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA

X, y = make_classification(
    n_samples=900, n_features=20, n_informative=7,
    n_redundant=8, n_repeated=0, random_state=42
)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

pca = PCA(n_components=0.90, random_state=42)
X_train_pca = pca.fit_transform(X_train_scaled)
X_test_pca = pca.transform(X_test_scaled)
print("Components retained:", pca.n_components_)
print("Cumulative explained variance:", pca.explained_variance_ratio_.sum())
```

Use the scaler fitted on training data to transform test data. Do not fit separate scalers or PCA objects to the test set.

### Visualize and choose components

```python
pca_2d = PCA(n_components=2)
X_2d = pca_2d.fit_transform(X_train_scaled)
plt.scatter(X_2d[:, 0], X_2d[:, 1], c=y_train, s=12, alpha=0.7)
plt.xlabel("PC1")
plt.ylabel("PC2")
plt.title("Training data projected onto first two PCs")
plt.show()

full_pca = PCA().fit(X_train_scaled)
cum = np.cumsum(full_pca.explained_variance_ratio_)
plt.plot(np.arange(1, len(cum)+1), cum, marker="o")
plt.axhline(0.90, linestyle="--")
plt.xlabel("Number of components")
plt.ylabel("Cumulative explained variance")
plt.show()
n_for_90 = np.searchsorted(cum, 0.90) + 1
print("Components for at least 90%:", n_for_90)
```

The minimum count is the first cumulative ratio at or above 0.90. This threshold is a heuristic; validate predictive usefulness separately.

## t-SNE (19–21)

```python
from sklearn.manifold import TSNE
from sklearn.datasets import make_blobs

X_vis, y_vis = make_blobs(n_samples=350, centers=4, n_features=8,
                          cluster_std=2.0, random_state=42)
X_vis = StandardScaler().fit_transform(X_vis)
embedding = TSNE(
    n_components=2, perplexity=30, init="pca",
    learning_rate="auto", max_iter=1000, random_state=42
).fit_transform(X_vis)

plt.scatter(embedding[:, 0], embedding[:, 1], c=y_vis, s=14)
plt.title("t-SNE exploratory embedding")
plt.show()
```

Perplexity must be smaller than the sample count. Compare reasonable values and seeds, but do not pick an embedding solely because it visually matches expected labels. Labels here are only used to color the plot.

## TruncatedSVD and random projection (22–23)

```python
from sklearn.decomposition import TruncatedSVD
from sklearn.random_projection import GaussianRandomProjection
from scipy.sparse import csr_matrix

rng = np.random.default_rng(42)
X_sparse = csr_matrix(rng.poisson(0.15, size=(300, 120)))
svd = TruncatedSVD(n_components=10, random_state=42)
X_svd = svd.fit_transform(X_sparse)
print(X_sparse.shape, X_svd.shape)

rp = GaussianRandomProjection(n_components=10, random_state=42)
X_projected = rp.fit_transform(X_sparse)
print(X_projected.shape)
```

Random projection is fast and generally not intended to produce interpretable axes. Evaluate whether distances or downstream performance are sufficiently preserved.

## Pipeline and downstream comparison (24–26)

```python
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import StratifiedKFold, cross_validate

without_pca = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler()),
    ("model", LogisticRegression(max_iter=2000)),
])
with_pca = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler()),
    ("pca", PCA(n_components=0.90, random_state=42)),
    ("model", LogisticRegression(max_iter=2000)),
])
cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
for name, estimator in [("No PCA", without_pca), ("PCA", with_pca)]:
    scores = cross_validate(estimator, X_train, y_train, cv=cv,
                             scoring=["accuracy", "f1", "roc_auc"])
    print(name, {m: scores[f"test_{m}"].mean()
                 for m in ["accuracy", "f1", "roc_auc"]})
```

Putting PCA in the pipeline makes it refit within each fold and prevents validation-fold information from influencing component directions. Fit the selected model on all training data, then evaluate on the untouched test set.

### Text features
Use TF-IDF or another sparse text representation, then apply `TruncatedSVD` (often called latent semantic analysis in this context). Evaluate downstream text classification/retrieval and retain enough components for the task.

### Component interpretation
Each component mixes multiple original features. Review component loadings, but remember that signs can flip and components may not map cleanly to one business concept.

## Advanced answers (29–36)
29. PCA optimizes total feature variance, not target prediction. A low-variance feature can carry a strong predictive signal and may be discarded when retaining only high-variance components.
30. PCA captures variance and has loadings that can be inspected; random projection is often fast and has less direct interpretability. Both should be assessed against task requirements.
31. Distances can become less informative in high dimensions, and irrelevant features can distort neighborhoods. Feature selection or justified reduction may help.
32. Repeat the embedding with multiple seeds and parameter settings; compare neighborhood preservation or trustworthiness and whether conclusions remain consistent.
33. Keep a test set aside, place scaling and reduction inside the training/CV pipeline, choose settings using training folds, and evaluate once on test data.
34. Embeddings may encode sensitive attributes or proxies. Minimize data, control access, assess subgroup performance, and validate intended use and retention policies.
35. t-SNE is optimized to preserve local neighborhood probabilities, not to prove statistical clusters. Visual islands can be artifacts of settings or sampling.
36. Monitor input distributions, missingness, embedding distributions, downstream error when labels arrive, and changes in population. Version the reducer and preprocessing steps.

## Mini-project outline (37–38)

```python
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import classification_report, roc_auc_score

X, y = make_classification(
    n_samples=1200, n_features=30, n_informative=8,
    n_redundant=12, random_state=42
)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, stratify=y, random_state=42
)
model = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler()),
    ("pca", PCA(n_components=0.90, random_state=42)),
    ("classifier", LogisticRegression(max_iter=2000)),
])
model.fit(X_train, y_train)
pred = model.predict(X_test)
proba = model.predict_proba(X_test)[:, 1]
print(classification_report(y_test, pred))
print("Test ROC-AUC:", roc_auc_score(y_test, proba))
print("Retained PCA components:", model.named_steps["pca"].n_components_)
```

For a fair comparison, fit a second pipeline without PCA and compare both using the same folds and held-out test rows. Do not use `y_test` to select component count. For visualization, fit 2D PCA and t-SNE embeddings to a scoped dataset; label plots as exploratory and do not treat plot separation as proof of cluster validity.
