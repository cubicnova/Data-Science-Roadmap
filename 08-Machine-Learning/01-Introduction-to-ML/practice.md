# Practice - Introduction to Machine Learning

Work through the questions in order. Try to answer each question before checking `solution.md`.

## Part A: Concepts

1. Explain machine learning in your own words.
2. Give two real-world examples of classification and two examples of regression.
3. What is the difference between supervised and unsupervised learning?
4. What is reinforcement learning? Give one possible application.
5. In a dataset containing `area_sqft`, `bedrooms`, and `price`, identify the features and target if the goal is to predict price.
6. Explain the difference between a model and an algorithm.
7. What are training and test sets, and why should the test set be held out?
8. Define overfitting and underfitting.
9. What is data leakage? Describe one way it can occur.
10. Why might accuracy be a poor metric for a highly imbalanced dataset?

## Part B: Identify the Task

For each scenario, identify the likely task type: classification, regression, clustering, or reinforcement learning.

1. Predict whether a customer will cancel a subscription.
2. Estimate tomorrow's electricity demand in kWh.
3. Group shoppers by purchasing behavior without existing group labels.
4. Train a game-playing agent using rewards for successful moves.
5. Predict whether a network event is benign or suspicious.
6. Estimate delivery time in minutes.

## Part C: Python and Scikit-learn

### Exercise 1 - Explore a Dataset

Using `sklearn.datasets.load_iris`:

1. Load the Iris dataset.
2. Print the number of samples and features.
3. Print the feature names and target names.
4. Display the first five rows of the feature matrix.
5. Count how many observations belong to each target class.

### Exercise 2 - Train/Test Split

1. Split Iris data into 80% training and 20% testing.
2. Use `random_state=42` and preserve class proportions with `stratify=y`.
3. Print the shape of each split.
4. Explain why stratification can be useful for classification.

### Exercise 3 - Train a Classifier

1. Train a `LogisticRegression` model on the training data.
2. Use `max_iter=1000` and `random_state=42`.
3. Predict labels for the test set.
4. Calculate accuracy.
5. Print a classification report and confusion matrix.

### Exercise 4 - Compare with a Baseline

1. Train a `DummyClassifier` using the most-frequent strategy.
2. Compare its test accuracy with Logistic Regression.
3. Explain why a baseline is useful.
4. Explain why a higher accuracy does not always mean a model is suitable for production.

### Exercise 5 - First Regression Model

1. Load the diabetes dataset using `load_diabetes()`.
2. Split the data into training and test sets.
3. Train `LinearRegression`.
4. Predict the target values on the test set.
5. Calculate mean absolute error (MAE) and R².
6. Explain what a lower MAE generally indicates and what a negative R² can mean.

## Part D: Mini-Projects

### Mini-Project 1 - Iris Classification

Create a complete classification workflow using the Iris dataset. Include:
- Problem statement
- Feature and target definitions
- Train/test split
- Baseline and chosen model
- Accuracy, confusion matrix, and classification report
- A short conclusion about limitations

### Mini-Project 2 - Regression Evaluation

Use the diabetes dataset to compare `LinearRegression` with a `DummyRegressor(strategy="mean")`. Compare MAE and R² on the same held-out test set. Explain why the baseline matters.

### Mini-Project 3 - Model Thinking

Choose a problem from education, retail, cybersecurity, healthcare, or transport. Write one page describing:
- The prediction or discovery goal
- Available data and possible features
- Target label, if supervised
- Which ML task applies
- One suitable metric
- Two risks, such as biased data, privacy concerns, leakage, or poor generalization

## Submission Checklist

- [ ] Code runs from top to bottom.
- [ ] Training and testing data are separate.
- [ ] Random states are set where appropriate for reproducibility.
- [ ] Metrics are calculated on held-out data.
- [ ] Results are explained in plain language.
- [ ] Limitations and potential data issues are acknowledged.
