# 01 - Introduction to Machine Learning

Welcome to **Introduction to Machine Learning**, part of the [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap).

This module introduces the core ideas behind machine learning and demonstrates how to build a first predictive model using Python and Scikit-learn.

## Learning Objectives

By the end of this module, you should be able to:

- Explain what machine learning is and where it is useful.
- Distinguish supervised, unsupervised, and reinforcement learning.
- Explain features, targets, samples, training data, and test data.
- Differentiate classification from regression.
- Describe a typical machine learning workflow.
- Load a dataset, split it into training and test sets, train a model, and make predictions.
- Evaluate a basic classification model using common metrics.
- Recognize overfitting, underfitting, data leakage, and the importance of a baseline.

## Prerequisites

- Basic Python: variables, lists, functions, loops, and imports.
- Basic familiarity with NumPy and pandas is helpful.
- Basic statistics is useful but not required.

## Setup

Use Python 3 and install the libraries:

```bash
python -m pip install numpy pandas matplotlib scikit-learn jupyter
```

Start Jupyter:

```bash
jupyter notebook
```

Open `examples.ipynb` and run the cells from top to bottom. The examples use datasets bundled with Scikit-learn, so no separate dataset download is needed.

## What Is Machine Learning?

Machine learning (ML) is a branch of computing in which algorithms learn patterns from data and use those patterns to make predictions, classifications, or discover structure. A model is the learned representation produced by a training algorithm.

Examples include:

- Predicting house prices.
- Classifying an email as spam or not spam.
- Grouping customers with similar behavior.
- Detecting unusual transactions.
- Recommending products or content.

A model does not automatically understand the world or guarantee correct results. Its performance depends on the quality and representativeness of the data, the problem definition, and the evaluation method.

## Main Types of Machine Learning

### 1. Supervised Learning

The training data includes input features (`X`) and a known target (`y`).

- **Classification:** predict a category, such as spam/not spam or a flower species.
- **Regression:** predict a numeric value, such as a price or temperature.

Common introductory algorithms include linear regression, logistic regression, decision trees, k-nearest neighbors, and random forests.

### 2. Unsupervised Learning

The data has input features but no provided target labels. The goal may be to discover structure.

Examples:
- **Clustering:** group similar customers using K-Means.
- **Dimensionality reduction:** summarize many features using PCA.
- **Anomaly detection:** identify unusual observations.

### 3. Reinforcement Learning

An agent learns by interacting with an environment and receiving rewards or penalties. It is commonly discussed in robotics, game-playing, and sequential decision-making.

## Important Terminology

| Term | Meaning |
|---|---|
| Sample / observation | One row or example in a dataset |
| Feature (`X`) | An input variable used by a model |
| Target / label (`y`) | The value or category to predict |
| Training set | Data used to fit the model |
| Test set | Held-out data used for final evaluation |
| Model | The learned mapping or pattern |
| Algorithm | The procedure used to learn a model |
| Parameter | A value learned during training |
| Hyperparameter | A configuration chosen before/during model selection |
| Prediction | A model's output for input data |
| Generalization | Performance on new, unseen data |
| Overfitting | Learning training-specific patterns that do not generalize well |
| Underfitting | Failing to learn important patterns in the data |
| Data leakage | Information unavailable at prediction time improperly influences training or evaluation |

## A Typical Machine Learning Workflow

1. Define the problem and the success criteria.
2. Collect and understand the data.
3. Clean and prepare the features.
4. Separate features (`X`) from target (`y`).
5. Split the data into training and test sets.
6. Establish a simple baseline.
7. Select and train a suitable model.
8. Evaluate on held-out data using metrics suited to the task.
9. Inspect errors and improve the pipeline.
10. Deploy, monitor, and maintain the model when appropriate.

**Important:** split data before fitting preprocessing steps. Fit transformations on training data only, then apply them to test data. Scikit-learn pipelines help reduce accidental data leakage.

## Classification vs. Regression

| Aspect | Classification | Regression |
|---|---|---|
| Output | Class/category | Numeric value |
| Example | Predict species | Predict price |
| Example metrics | Accuracy, precision, recall, F1 | MAE, MSE/RMSE, R² |
| Introductory model | Logistic Regression | Linear Regression |

Choose metrics based on the real-world cost of errors. Accuracy alone can be misleading for imbalanced classification problems.

## Repository Contents

```text
01-Introduction-to-ML/
├── README.md
├── examples.ipynb
├── practice.md
└── solution.md
```

## Learning Path

1. Read the concepts in this README.
2. Run `examples.ipynb` in order.
3. Attempt the questions in `practice.md` without looking at the solutions.
4. Compare your work with `solution.md`.
5. Modify the examples and explain what changes in the results.

## Further Reading

- [Scikit-learn: Getting Started](https://scikit-learn.org/stable/getting_started.html)
- [Scikit-learn: User Guide](https://scikit-learn.org/stable/user_guide.html)
- [Scikit-learn: Choosing the Right Estimator](https://scikit-learn.org/stable/machine_learning_map.html)

## Next Topic

Continue to the next module in `08-Machine-Learning` to build on these fundamentals with more focused algorithms and workflows.
