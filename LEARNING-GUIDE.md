# Learning Guide

Welcome to the **CubicNova Data Science Roadmap** — a structured, hands-on path for learning data science, machine learning, deep learning, and practical AI engineering.

This guide explains how to navigate the repository, what order to study topics in, how to practice effectively, and how to turn your learning into reproducible projects.

> **Learning principle:** Learn a concept, implement it, practice it, explain it in your own words, and apply it to a project. Do not measure progress only by the number of tutorials completed.

## Table of Contents

- [1. How to Use This Repository](#1-how-to-use-this-repository)
- [2. Recommended Learning Sequence](#2-recommended-learning-sequence)
- [3. Suggested Pace](#3-suggested-pace)
- [4. Module Study Workflow](#4-module-study-workflow)
- [5. Learning Stages](#5-learning-stages)
- [6. Practice with Sample Datasets](#6-practice-with-sample-datasets)
- [7. Project-Based Learning](#7-project-based-learning)
- [8. Building a Research Habit](#8-building-a-research-habit)
- [9. Interview Preparation](#9-interview-preparation)
- [10. Progress Tracker](#10-progress-tracker)
- [11. Recommended Weekly Routine](#11-recommended-weekly-routine)
- [12. Common Mistakes to Avoid](#12-common-mistakes-to-avoid)
- [13. Completion Checklist](#13-completion-checklist)
- [14. Useful Resources](#14-useful-resources)
- [15. Contributing](#15-contributing)

---

## 1. How to Use This Repository

The repository is organized into numbered sections so you can progress from fundamentals to specialized and production-oriented topics. Folder names and available files may evolve; use the repository's root `README.md` and `ROADMAP.md` as the source of truth for the current structure.

### Recommended approach

1. Start with the prerequisites and setup instructions.
2. Follow the numbered modules in sequence unless you already know a topic well.
3. Read the module's `README.md` to understand its learning objectives.
4. Run the code examples yourself and change the inputs.
5. Complete the exercises in `practice.md`.
6. Attempt exercises before reading `solution.md`.
7. Use `examples.ipynb` for hands-on experimentation when the notebook is provided.
8. Build a small project after completing a group of related topics.
9. Record what you learned, what failed, and what you want to revisit.
10. Review and improve earlier work as your skills develop.

### Typical module structure

Where provided, learning modules use this structure:

```text
module-name/
├── README.md
├── examples.ipynb
├── practice.md
└── solution.md
```

- **`README.md`** — objectives, concepts, explanations, examples, and references.
- **`examples.ipynb`** — executable demonstrations and guided experiments.
- **`practice.md`** — exercises and questions to solve independently.
- **`solution.md`** — worked answers for checking your understanding.

Not every directory necessarily contains all four files. Follow the files that are available in the relevant module.

### Set up your environment

Clone the repository:

```bash
git clone https://github.com/cubicnova/Data-Science-Roadmap.git
cd Data-Science-Roadmap
```

Create a virtual environment.

**Windows PowerShell:**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
```

**Linux or macOS:**

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
```

Install commonly used data science libraries when needed:

```bash
python -m pip install numpy pandas matplotlib seaborn scikit-learn jupyter
```

Install additional packages such as PyTorch, TensorFlow, XGBoost, or other tools only when a module requires them. Check the relevant official installation instructions and hardware requirements.

Start Jupyter:

```bash
jupyter notebook
```

You can also use JupyterLab or Visual Studio Code with its Python and Jupyter extensions.

---

## 2. Recommended Learning Sequence

The repository's numbered sections provide the detailed topic structure. Use the following overview to understand how the stages connect.

| Stage | Repository section | Main goal |
|---|---|---|
| 1 | `00-Getting-Started` | Understand the roadmap, tools, environment, and learning workflow |
| 2 | Python and data fundamentals modules | Write readable Python and understand common data structures |
| 3 | Mathematics and statistics modules | Build the mathematical foundations for analysis and modeling |
| 4 | NumPy and Pandas modules | Work efficiently with arrays and tabular data |
| 5 | SQL, visualization, and EDA modules | Retrieve, inspect, summarize, and communicate insights from data |
| 6 | `08-Machine-Learning` | Learn supervised and unsupervised learning workflows |
| 7 | `09-Advanced-Machine-Learning` | Explore advanced models, interpretability, forecasting, and specialized problems |
| 8 | `10-Deep-Learning-and-AI` | Learn neural networks, computer vision, NLP, transformers, and generative AI |
| 9 | Data engineering, deployment, and MLOps modules | Package, deploy, track, test, and monitor data and ML systems |
| 10 | `13-Interview-Preparation` | Review concepts, solve coding challenges, and practice case studies |
| 11 | `14-Cheatsheets` and `15-Resources` | Revise key concepts and find documentation, datasets, and research papers |

**Navigation note:** Use the actual folder names in the repository if a section has been renamed or reorganized. The sequence above is a learning guide, not a guarantee that every named topic is complete.

## 3. Suggested Pace

The following schedule is a flexible starting point. Adjust it to your current experience, available time, and target role.

| Weeks | Focus | Expected outcome |
|---|---|---|
| 1–2 | Setup, Python fundamentals, Git, notebooks | Write and run basic Python programs |
| 3–5 | Statistics, probability, NumPy, Pandas | Analyze and summarize datasets |
| 6–7 | SQL, visualization, exploratory data analysis | Complete a documented data analysis |
| 8–12 | Core machine learning | Train and evaluate baseline models |
| 13–16 | Advanced ML and deep learning fundamentals | Compare more advanced methods and understand their trade-offs |
| 17–20 | Choose a specialization and build a project | Produce a reproducible, end-to-end project |
| 21+ | Deployment, MLOps, research, and interviews | Improve project quality and prepare for target roles |

These timings are illustrative. If you are new to programming or mathematics, spend longer on the foundations. If you already have experience, review familiar topics and move forward after demonstrating practical understanding.

### Define a milestone for every stage

A stage is complete when you can:

- Explain the main concepts without copying a definition.
- Write or modify a working example independently.
- Complete the associated practice tasks.
- Identify common errors and limitations.
- Apply the concept to a small, relevant problem.

---

## 4. Module Study Workflow

Use the same workflow for most modules.

### Step 1: Preview

Read the module objectives and table of contents. Identify what you should be able to do by the end.

### Step 2: Learn

Read the explanations and consult the linked documentation. Focus on understanding the reason behind a technique, not just its syntax.

### Step 3: Run the examples

Execute code one section at a time. Change a parameter, use a different input, and observe what happens.

### Step 4: Practice independently

Complete the exercises before opening the solutions. If stuck, break the problem into smaller steps and consult documentation before looking at a complete answer.

### Step 5: Explain

Write a short explanation of the concept, its use cases, assumptions, and limitations. If you cannot explain why an approach works, revisit the underlying idea.

### Step 6: Apply

Use the concept in a small analysis or project. Keep the code, results, and instructions organized so another person can reproduce the work.

### Step 7: Review

At the end of the week, revisit difficult concepts and solve a few exercises without looking at your notes.

### Keep a learning log

A simple log can use the following format:

```markdown
# Weekly Learning Log

## Topics completed
- 

## What I can now do independently
- 

## Exercises or experiments completed
- 

## Errors I encountered and how I fixed them
- 

## Concepts to revisit
- 

## Next week's goal
- 
```

---

## 5. Learning Stages

### Stage A: Programming and Data Fundamentals

**Focus:** Python syntax, variables, data types, conditions, loops, functions, collections, file handling, modules, and basic Git.

**Practice:**
- Write small functions and test them with different inputs.
- Read and write CSV and JSON files.
- Use exceptions to handle invalid inputs.
- Break a program into reusable functions.
- Commit meaningful changes to Git.

**Milestone:** Create a small Python program that reads a data file, validates inputs, calculates summary information, and prints a clear report.

### Stage B: Mathematics, Statistics, and Probability

**Focus:** Descriptive statistics, probability, distributions, sampling, confidence intervals, hypothesis testing, correlation, linear algebra, and optimization fundamentals.

**Practice:**
- Calculate mean, median, variance, and standard deviation.
- Simulate simple probability experiments.
- Investigate correlation and misleading interpretations.
- Explain the difference between correlation and causation.
- Interpret a confidence interval in context.

**Milestone:** Analyze a dataset and explain its key statistical properties, uncertainty, and limitations.

### Stage C: NumPy, Pandas, and Data Preparation

**Focus:** Arrays, indexing, vectorized operations, DataFrames, joins, grouping, missing values, data types, duplicate records, date handling, and feature preparation.

**Practice:**
- Load multiple files and compare their schemas.
- Clean missing and inconsistent values.
- Group data and calculate useful metrics.
- Merge related tables.
- Create a repeatable data-cleaning script.

**Milestone:** Turn a raw dataset into a documented, analysis-ready dataset.

### Stage D: SQL, Visualization, and EDA

**Focus:** SQL filtering, joins, aggregation, subqueries, common table expressions, chart selection, distributions, relationships, and analytical storytelling.

**Practice:**
- Answer business questions using SQL.
- Plot distributions and relationships.
- Investigate outliers and missingness.
- Summarize findings in plain language.
- Distinguish observed patterns from causal claims.

**Milestone:** Publish an EDA report with meaningful visualizations, evidence-backed findings, and caveats.

### Stage E: Core Machine Learning

**Focus:** Regression, classification, clustering, preprocessing, train-test splits, cross-validation, model metrics, feature engineering, pipelines, and hyperparameter tuning.

**Practice:**
- Build a simple baseline before a complex model.
- Compare models using appropriate metrics.
- Use cross-validation for model selection.
- Fit preprocessing only on training data through a proper pipeline.
- Inspect errors and explain the model's limitations.

**Milestone:** Build an end-to-end ML workflow with reproducible evaluation and a clear explanation of its results.

### Stage F: Advanced Machine Learning

**Focus:** Ensemble methods, gradient boosting, imbalanced datasets, dimensionality reduction, anomaly detection, model interpretability, and time-series forecasting.

**Practice:**
- Compare a baseline with an advanced method.
- Investigate the effect of class imbalance.
- Evaluate a forecasting model with time-aware splits.
- Use feature attribution carefully.
- Record performance, runtime, and complexity.

**Milestone:** Complete a comparison study that explains not only which model performed better but also under what conditions and at what cost.

### Stage G: Deep Learning and AI

**Focus:** Neural networks, optimization, CNNs, sequence models, NLP, transformers, computer vision, and generative AI.

**Practice:**
- Train a small neural network.
- Use transfer learning where appropriate.
- Track training and validation behavior.
- Investigate overfitting and regularization.
- Evaluate errors and failure cases.

**Milestone:** Build a small deep learning project and document its data, architecture, evaluation, and limitations.

### Stage H: Specialization, Deployment, and MLOps

Choose a direction based on your goals, such as NLP, computer vision, time series, recommendation systems, or production ML.

**Focus:** APIs, packaging, experiment tracking, data and model versioning, testing, deployment, monitoring, and reproducibility.

**Practice:**
- Package model inference behind a clear interface.
- Record dependencies and model versions.
- Add basic input validation and tests.
- Document how to reproduce the experiment.
- Consider drift, latency, resource usage, and rollback needs.

**Milestone:** Deliver a project that another person can set up, run, test, and understand.

---

## 6. Practice with Sample Datasets

The repository's `datasets/sample/` directory is intended to support hands-on learning. Use the files that are actually present in that directory.

| Dataset file | Suggested exercise | Skills |
|---|---|---|
| `students.csv` | Analyze attendance, study hours, and exam scores | Pandas, grouping, visualization |
| `sales.csv` | Calculate revenue by product, date, and channel | Aggregation, dates, EDA |
| `employees.csv` | Summarize salary and experience by department | Grouping, distributions, categorical analysis |
| `house_prices.csv` | Build a baseline property-price model | Regression, feature engineering, evaluation |
| `iris.csv` | Classify flower species | Classification, metrics, visualization |
| `customer_churn.csv` | Explore churn and build a baseline classifier | Classification, class imbalance, precision and recall |
| `weather.csv` | Plot changes in weather measurements over time | Datetime handling, visualization, time series |
| `products.json` | Load and filter product records | JSON, structured data, transformations |

These examples are educational. Check the dataset README and data dictionary for provenance, definitions, and limitations. Synthetic datasets are not evidence of real-world behavior.

### Example: Start an EDA session

```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv("datasets/sample/sales.csv")

print("Shape:", df.shape)
print(df.head())
print(df.info())
print(df.isna().sum())

df["date"] = pd.to_datetime(df["date"])

revenue_by_product = (
    df.groupby("product")["revenue_inr"]
      .sum()
      .sort_values(ascending=False)
)

print(revenue_by_product)

revenue_by_product.plot(kind="bar", title="Revenue by Product")
plt.xlabel("Product")
plt.ylabel("Revenue (INR)")
plt.tight_layout()
plt.show()
```

Run code from the repository root or adjust the path to match your current working directory. Verify column names against the actual file before running an example.

### Data science practice checklist

- [ ] Inspect the shape, schema, and data types.
- [ ] Check missing values and duplicates.
- [ ] Validate units and value ranges.
- [ ] Document cleaning decisions.
- [ ] Explore distributions and relationships.
- [ ] Formulate questions before modeling.
- [ ] Keep training and test data separate.
- [ ] Explain findings and uncertainty.

---

## 7. Project-Based Learning

Projects help connect individual techniques into a complete workflow. Start small, then add complexity only when it improves the solution.

### Beginner projects

- Student performance analysis.
- Sales trend analysis.
- Employee data exploration.
- Product catalog analysis.
- Interactive data visualization report.

### Intermediate projects

- House-price regression.
- Customer-churn classification.
- Customer segmentation.
- Anomaly detection.
- Time-series forecasting.

### Advanced projects

- Image classification with transfer learning.
- NLP text classification.
- Recommendation system.
- Retrieval-augmented generation evaluation.
- Reproducible model training and deployment pipeline.

### Recommended project structure

```text
project-name/
├── README.md
├── requirements.txt
├── data/
│   └── README.md
├── notebooks/
│   └── exploration.ipynb
├── src/
│   ├── data.py
│   ├── features.py
│   ├── train.py
│   └── evaluate.py
├── tests/
├── reports/
└── models/
    └── README.md
```

This is an example structure, not a requirement for every project. Avoid committing large datasets, secrets, private information, or model artifacts that should instead be downloaded or generated separately.

### What a good project README should explain

1. Problem statement and intended user.
2. Dataset source, license, and limitations.
3. Data preparation and feature engineering.
4. Baseline and chosen method.
5. Evaluation procedure and metrics.
6. Results and error analysis.
7. How to install and run the project.
8. Known limitations and next steps.

Do not report only a score. Explain what the score means, how it was measured, and where the system can fail.

---

## 8. Building a Research Habit

Research papers can deepen your understanding after you have learned the basic concepts.

Use the repository's `15-Resources/Research-Papers.md` and `15-Resources/Official-Documentation.md` when available.

A practical workflow:

1. Choose a topic you have already studied.
2. Read an introductory or foundational paper.
3. Identify the problem, proposed method, assumptions, and evaluation.
4. Compare the paper's method with a simpler baseline.
5. Check whether code and datasets are available.
6. Reproduce a small experiment when feasible.
7. Record deviations and limitations.

Use official documentation to understand library behavior and research papers to understand methods, evidence, and trade-offs. Verify publication details and licensing before reusing research code or datasets.

---

## 9. Interview Preparation

Interview preparation should run alongside practical learning rather than being postponed until the end.

Use `13-Interview-Preparation/` and the relevant cheatsheets and resources when available.

### Core areas to review

- Python data structures, functions, and problem-solving.
- SQL joins, aggregation, window functions, and query reasoning.
- Statistics, probability, sampling, and hypothesis testing.
- Data cleaning and exploratory analysis.
- Machine learning algorithms and their assumptions.
- Metrics, cross-validation, leakage, and overfitting.
- Feature engineering and model interpretation.
- Deep learning fundamentals for relevant roles.
- Case studies and communicating results.
- Project design, reproducibility, and deployment concepts.

### Weekly interview practice

- Solve a few coding or SQL problems.
- Explain one algorithm without reading notes.
- Interpret a model evaluation scenario.
- Review one project and its trade-offs.
- Practice communicating a technical result to a non-technical audience.

For case studies, clarify the goal, data availability, constraints, success metric, baseline, evaluation strategy, risks, and how the solution would be monitored.

---

## 10. Progress Tracker

Use this checklist to monitor practical capability rather than merely marking pages as read.

### Foundations

- [ ] I can use Python to write small, testable programs.
- [ ] I can work with Git and a virtual environment.
- [ ] I understand basic probability and descriptive statistics.
- [ ] I can load, clean, and inspect a dataset.
- [ ] I can use SQL to filter and aggregate structured data.

### Analysis

- [ ] I can perform EDA and create meaningful visualizations.
- [ ] I can document data-quality issues.
- [ ] I can explain findings without overstating causality.
- [ ] I can create a clear analysis report.

### Machine learning

- [ ] I can establish a simple baseline.
- [ ] I can choose metrics appropriate to the problem.
- [ ] I can use validation without leaking information.
- [ ] I can compare models fairly.
- [ ] I can investigate errors and model limitations.

### Projects and communication

- [ ] I have completed at least one documented project.
- [ ] Another person can reproduce my work from the instructions.
- [ ] I can explain my design decisions and trade-offs.
- [ ] I can describe what I would improve next.

### Advanced practice

- [ ] I have explored at least one specialization.
- [ ] I understand the basics of model packaging and deployment.
- [ ] I can read and summarize a research paper.
- [ ] I have practiced relevant interview questions and case studies.

---

## 11. Recommended Weekly Routine

Adapt this sample to your schedule. Consistency is more important than following a rigid number of hours.

| Session | Activity |
|---|---|
| Session 1 | Learn a new concept and take concise notes |
| Session 2 | Run examples and modify the code |
| Session 3 | Complete exercises independently |
| Session 4 | Apply the concept to a dataset or project |
| Session 5 | Review mistakes, summarize learning, and plan next steps |

For each study session, define a small outcome such as "complete three Pandas exercises" rather than "study Pandas."

At the end of each week, ask:

- What can I now do without a tutorial?
- What mistakes did I make repeatedly?
- What evidence shows that I understand the topic?
- What is the next smallest useful task?

---

## 12. Common Mistakes to Avoid

- **Skipping fundamentals:** Build programming and statistics skills before relying heavily on advanced models.
- **Watching without practicing:** Write code and solve exercises yourself.
- **Copying solutions too early:** Attempt the problem and understand the reasoning before checking the answer.
- **Chasing tools:** Learn the problem-solving concepts before switching between libraries.
- **Using inappropriate metrics:** Select evaluation measures based on the task and real-world cost of errors.
- **Data leakage:** Keep test data isolated and fit transformations only on training data.
- **Overfitting to a test set:** Use validation or cross-validation for model selection.
- **Ignoring baselines:** Compare complex models with simple, meaningful alternatives.
- **Reporting only the best result:** Include limitations, failed experiments, and variability where relevant.
- **Neglecting documentation:** Explain how to install, run, and reproduce your project.
- **Using data without checking its source:** Review license, provenance, privacy, and permitted use.
- **Trying to learn everything at once:** Complete a foundation, apply it, then expand.

---

## 13. Completion Checklist

You do not need to master every topic in data science before building useful projects. Use this checklist to evaluate whether you can apply the foundations independently.

- [ ] I can write Python code and manage a basic development environment.
- [ ] I can clean, analyze, and visualize tabular data.
- [ ] I understand the statistical concepts relevant to my analysis.
- [ ] I can use SQL for common data retrieval tasks.
- [ ] I can train and evaluate a baseline machine learning model.
- [ ] I understand validation, leakage, overfitting, and suitable metrics.
- [ ] I have completed and documented a project.
- [ ] I can explain results, assumptions, and limitations.
- [ ] I can use official documentation to solve unfamiliar problems.
- [ ] I have a plan for the next skill or specialization to develop.

Treat completion as the beginning of continued practice, not a claim that the entire field has been mastered.

---

## 14. Useful Resources

Use the repository's resource section as your main index, and consult official documentation when implementing a library or method.

- [Python Documentation](https://docs.python.org/3/)
- [NumPy Documentation](https://numpy.org/doc/stable/)
- [Pandas Documentation](https://pandas.pydata.org/docs/)
- [Matplotlib Documentation](https://matplotlib.org/stable/)
- [Seaborn Documentation](https://seaborn.pydata.org/)
- [Scikit-learn User Guide](https://scikit-learn.org/stable/user_guide.html)
- [PyTorch Documentation](https://pytorch.org/docs/stable/)
- [TensorFlow Guides](https://www.tensorflow.org/guide)
- [UCI Machine Learning Repository](https://archive.ics.uci.edu/)
- [Kaggle Datasets](https://www.kaggle.com/datasets)
- [Google Dataset Search](https://datasetsearch.research.google.com/)
- [arXiv](https://arxiv.org/)
- [Papers with Code](https://paperswithcode.com/)

Check each project's current documentation for installation instructions, compatibility, and supported versions.

## 15. Contributing

Help improve the roadmap by contributing clear explanations, runnable examples, well-designed practice exercises, corrected links, and useful project ideas.

Before submitting a contribution:

- [ ] Confirm the content belongs in the relevant module.
- [ ] Follow existing naming and folder conventions.
- [ ] Test code examples where practical.
- [ ] Keep explanations accurate and beginner-friendly.
- [ ] Cite original documentation or research when appropriate.
- [ ] Check links, paths, and dataset instructions.
- [ ] Avoid committing credentials, personal data, or restricted datasets.
- [ ] Update related navigation files when the repository structure changes.

If you find a broken link or an unclear section, open an issue or propose a focused change.

---

## Final Advice

**Learn → Practice → Build → Explain → Review.**

Follow the roadmap at a sustainable pace. Focus on understanding, practical ability, and reproducible work. A small project that you can explain and improve is more valuable than a long list of topics you have only read.
