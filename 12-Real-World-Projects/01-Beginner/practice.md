# Practice: Beginner Real-World Projects

Use the synthetic dataset in `examples.ipynb` or choose an open dataset with a clear license. Keep your work reproducible.

## Exercise 1 — Frame the problem

Choose one project idea: student performance, retail sales, churn, house prices, or movie ratings.

Write:
- The practical question
- Who would use the answer
- The outcome you want to explain or predict
- Two limitations or risks

## Exercise 2 — Inspect and clean data

Create a data dictionary and report:
- Row and column counts
- Column types
- Missing values per column
- Duplicate rows
- Invalid or implausible values

Apply at least two cleaning steps and explain why each is appropriate. Avoid silently deleting large portions of the data.

## Exercise 3 — Exploratory data analysis

Create at least three visualizations appropriate to your dataset, such as:
- A distribution plot
- A category comparison
- A relationship or time-series plot

Give every chart a title, axis labels, and units where relevant. Write two evidence-based observations and one question that needs more investigation.

## Exercise 4 — Build a simple baseline model (optional for descriptive projects)

For a regression project, try `DummyRegressor` and a simple linear model. For a classification project, try `DummyClassifier` and logistic regression.

1. Split the data before fitting preprocessing steps.
2. Use a pipeline for transformations where appropriate.
3. Evaluate on held-out data.
4. Compare with a baseline and explain the chosen metric.

## Exercise 5 — Write an executive summary

Write 150–250 words containing:
- Problem and data source
- Main findings
- One chart or metric worth highlighting
- Limitations
- Recommended next step

Do not claim causation from correlation alone.

## Exercise 6 — Make the project reproducible

Create a `requirements.txt`, set random seeds where appropriate, use relative paths, and write setup instructions in a README. Add a `.gitignore` that excludes `.venv/`, `__pycache__/`, `.ipynb_checkpoints/`, and private/local data.

## Exercise 7 — Portfolio review

Ask a peer to reproduce your work from the README. Record any missing instructions. Remove personal or confidential information and confirm that the dataset's license allows your intended use.

## Reflection questions

1. What is the difference between descriptive analysis and prediction?
2. Why should data cleaning decisions be documented?
3. Why does correlation not imply causation?
4. What is a baseline model and why is it useful?
5. Why should the test set not be used repeatedly for tuning?
