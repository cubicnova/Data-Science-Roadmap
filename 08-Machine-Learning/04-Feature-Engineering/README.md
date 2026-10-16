# 04 — Feature Engineering

Feature engineering transforms raw data into useful, reliable inputs for machine learning models. This module explores how to create, transform, encode, select, and validate features while preventing data leakage.

## Learning objectives

By the end of this module, you should be able to:

- Explain why feature engineering can improve model quality and interpretability.
- Create meaningful numerical, categorical, date/time, and text-derived features.
- Handle missing values using appropriate strategies.
- Encode nominal and ordinal categorical variables correctly.
- Choose and apply scaling methods based on data and model needs.
- Transform skewed variables, bin continuous values, and create interactions.
- Identify and treat outliers with domain context.
- Use feature-selection methods without contaminating evaluation data.
- Build reproducible transformations with `Pipeline` and `ColumnTransformer`.
- Detect target leakage and train-serving inconsistencies.

## What is feature engineering?

Feature engineering is the process of using domain knowledge and data analysis to convert raw fields into representations that help a model learn. Good features can expose patterns, reduce noise, and make relationships easier for a model to represent. More features are not automatically better: irrelevant, duplicated, unstable, or leaked features can harm generalization.

## Topics

### 1. Feature creation
- Ratios, differences, counts, and aggregations.
- Interaction features and polynomial terms.
- Domain-derived features such as spend per visit or debt-to-income ratio.
- Avoid division-by-zero and invalid-value problems.

### 2. Missing-value handling
- Missingness inspection and reasons for missing data.
- Mean/median imputation for numerical variables.
- Most-frequent or explicit-category imputation for categorical variables.
- Missingness indicators where absence may itself carry information.
- Fit imputers on training data only.

### 3. Categorical encoding
- One-hot encoding for nominal categories.
- Ordinal encoding only when a meaningful order exists.
- Handling unseen categories at inference time.
- High-cardinality categories: consider domain-based grouping or carefully validated alternatives.

### 4. Scaling and normalization
- `StandardScaler`: centers features and scales to unit variance.
- `MinMaxScaler`: maps values to a chosen range.
- `RobustScaler`: uses robust statistics and may help when outliers are present.
- Scaling is important for many distance-based and gradient-based models; tree models often need less scaling.

### 5. Transformations and binning
- Log / power transforms for skewed positive data.
- Binning continuous values into interpretable ranges.
- Polynomial and interaction features for non-linear relationships.
- Transformation choices should be learned from training data and evaluated, not assumed to help.

### 6. Date/time and text features
- Extract year, month, day of week, hour, or elapsed time from dates.
- Use cyclical representations for periodic features when useful.
- Derive text length, token counts, or TF-IDF features.
- Ensure every feature is available at the time a prediction is made.

### 7. Outlier treatment
- Inspect ranges and use domain knowledge before removing observations.
- Use quantiles, IQR, robust statistics, or domain limits to flag potential outliers.
- Consider clipping/winsorization, robust scaling, transformation, or retaining valid extremes.
- Do not automatically delete an extreme value: it may be meaningful.

### 8. Feature selection
- Remove constant or near-constant features when appropriate.
- Use filter methods, mutual information, model-based selection, or recursive feature elimination.
- Compare selection methods within cross-validation.
- Feature importance is predictive evidence, not proof of causation.

### 9. Leakage-safe workflows
- Split before fitting transformations that learn from data.
- Put imputation, scaling, encoding, and feature selection inside a pipeline.
- Use the same transformation logic during training and inference.
- For time-series data, use time-aware splits and avoid future information.

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

Open `examples.ipynb` and run cells from top to bottom. The notebook generates synthetic data locally and does not require external datasets.

## Recommended workflow

1. Understand the prediction task, users, and decision time.
2. Inspect columns, data types, missingness, unique values, and suspicious fields.
3. Create a train/validation/test split appropriate to time and grouping.
4. Engineer features using information available at prediction time.
5. Build preprocessing steps inside a pipeline.
6. Compare against a baseline and evaluate with task-appropriate metrics.
7. Review feature stability, fairness, interpretability, and operational constraints.
8. Document transformations and monitor input distributions after deployment.

## Repository contents

- `README.md` — concepts, setup, and learning path.
- `examples.ipynb` — guided feature engineering examples.
- `practice.md` — exercises and mini-project.
- `solution.md` — reference answers and code explanations.

## Troubleshooting

- **Unknown categories at prediction time:** use encoders configured to handle unknown categories, such as `OneHotEncoder(handle_unknown="ignore")`.
- **Unexpected NaNs or infinities:** inspect division by zero, log of non-positive values, and invalid date parsing.
- **Model score drops after deployment:** compare training and serving feature definitions, missingness, category distributions, and data timing.
- **Suspiciously strong performance:** investigate duplicates, target-derived fields, post-outcome information, and preprocessing performed before splitting.
- **Feature count changes between training and inference:** keep encoders and transformations in a fitted pipeline rather than recreating them manually.

## Good-practice checklist

- [ ] Every feature is available at prediction time.
- [ ] Missing values and outliers are investigated, not blindly removed.
- [ ] Encoding reflects the meaning of each category.
- [ ] Scaling is selected based on the estimator and feature distribution.
- [ ] Learned transformations are fitted only on training folds.
- [ ] Feature selection is evaluated inside cross-validation.
- [ ] The final pipeline handles unseen categories and missing values.
- [ ] Feature definitions and limitations are documented.

## Further reading

- [scikit-learn: Preprocessing data](https://scikit-learn.org/stable/modules/preprocessing.html)
- [scikit-learn: Feature selection](https://scikit-learn.org/stable/modules/feature_selection.html)
- [scikit-learn: Pipelines and composite estimators](https://scikit-learn.org/stable/modules/compose.html)
