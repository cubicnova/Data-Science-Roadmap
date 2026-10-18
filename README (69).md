# CubicNova Data Science Roadmap

A structured, hands-on learning repository for building data science skills — from Python and data analysis fundamentals to machine learning, deep learning, generative AI, research, and practical projects.

This roadmap is designed for self-learners, students, analysts, aspiring data scientists, and developers who want to build a strong foundation through explanations, code examples, practice exercises, datasets, project work, and curated resources.

> **Learn the concept. Practice with data. Build a project. Explain your decisions. Repeat.**

## Table of Contents

- [About This Roadmap](#about-this-roadmap)
- [Who Is This For?](#who-is-this-for)
- [How to Use This Repository](#how-to-use-this-repository)
- [Roadmap Overview](#roadmap-overview)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
- [Sample Datasets](#sample-datasets)
- [Recommended Learning Workflow](#recommended-learning-workflow)
- [Project-Based Learning](#project-based-learning)
- [Research and Interview Preparation](#research-and-interview-preparation)
- [Learning Progress Checklist](#learning-progress-checklist)
- [Recommended Tools](#recommended-tools)
- [Learning Resources](#learning-resources)
- [Contributing](#contributing)
- [License](#license)

---

## About This Roadmap

The CubicNova Data Science Roadmap organizes learning into connected topics so you can progress from programming fundamentals to practical data science and AI applications.

The repository is intended to be more than a list of links. As the material develops, modules may include explanations, examples, notebooks, exercises, solutions, cheatsheets, datasets, research references, and project guidance.

Use the root-level [LEARNING-GUIDE.md](LEARNING-GUIDE.md) for a detailed study strategy and suggested learning sequence. Use [ROADMAP.md](ROADMAP.md), where available, as the topic-level navigation map.

## Who Is This For?

- **Beginners** who want a guided introduction to Python and data science.
- **Students** who need practical examples alongside theoretical concepts.
- **Data analysts** who want to strengthen statistics, Python, visualization, and machine learning skills.
- **Aspiring data scientists** who want a structured path and portfolio projects.
- **Developers and engineers** who want to explore machine learning, deep learning, and AI workflows.
- **Interview candidates** who want to revise concepts and practice coding and case studies.

You do not need to master every topic before starting a project. Learn the prerequisites for a task, build a small baseline, and extend it as your understanding grows.

## How to Use This Repository

1. **Start with the fundamentals.** Set up Python, Git, and a notebook environment.
2. **Follow the roadmap.** Work through the numbered topic folders in order, adjusting for prior experience.
3. **Run the examples.** Type or modify code instead of only reading it.
4. **Complete practice tasks.** Attempt exercises before checking reference solutions, when provided.
5. **Use the sample datasets.** Practice data inspection, cleaning, EDA, visualization, and basic modeling.
6. **Build projects.** Combine multiple concepts into a reproducible workflow.
7. **Use the cheatsheets for revision.** Refer to them after studying the relevant topic.
8. **Explore research and official documentation.** Go deeper when you understand the basics of a method.
9. **Track your progress.** Record what you can do independently and what needs review.
10. **Contribute improvements.** Fix unclear explanations, add examples, and report broken links.

Some folders may be under development, and not every module will contain the same set of files. Follow the content currently available in each directory.

## Roadmap Overview

The repository is organized into progressive learning areas. Folder names below reflect the established topic areas; consult the current directory listing for the exact available subfolders.

| Learning area | What you will learn |
|---|---|
| Getting Started and Environment | Repository navigation, Python installation, notebooks, tools, and workflow |
| Data Science Fundamentals | Data science lifecycle, problem framing, data types, and analytical thinking |
| Python for Data Science | Variables, collections, functions, files, modules, and reusable code |
| Mathematics, Statistics, and Probability | Descriptive statistics, probability, distributions, inference, and linear algebra |
| NumPy and Pandas | Arrays, DataFrames, data cleaning, transformation, and aggregation |
| Data Visualization and EDA | Data quality, distributions, relationships, charts, and communicating findings |
| SQL and Data Handling | Querying structured data, joins, aggregation, and data preparation |
| Machine Learning | Regression, classification, clustering, evaluation, preprocessing, and model selection |
| Advanced Machine Learning | Ensembles, boosting, dimensionality reduction, imbalanced data, anomaly detection, and forecasting |
| Deep Learning and AI | Neural networks, computer vision, NLP, transformers, and generative AI |
| Data Engineering and MLOps | Reproducibility, pipelines, experiment tracking, deployment, and monitoring |
| Interview Preparation | Technical questions, coding challenges, case studies, and revision |
| Cheatsheets | Concise syntax and concept references for quick review |
| Resources | Datasets, official documentation, research papers, and further learning materials |

### Core learning sequence

A practical order for most beginners is:

1. Python fundamentals and development environment.
2. Data structures, functions, files, and basic programming practice.
3. Mathematics, statistics, and probability.
4. NumPy and Pandas.
5. SQL, data cleaning, visualization, and exploratory data analysis.
6. Core machine learning and evaluation.
7. Advanced machine learning or a selected specialization.
8. Deep learning and AI topics as required by your goals.
9. Projects, deployment, MLOps, and reproducibility.
10. Interview preparation and continued learning.

For a more detailed study plan, read [LEARNING-GUIDE.md](LEARNING-GUIDE.md).

## Repository Structure

The following is a high-level guide to the main resource areas. It is not intended to replace the live folder listing.

```text
Data-Science-Roadmap/
├── README.md
├── LEARNING-GUIDE.md
├── ROADMAP.md
├── datasets/
│   └── sample/
│       ├── README.md
│       ├── data_dictionary.md
│       ├── students.csv
│       ├── sales.csv
│       ├── employees.csv
│       ├── house_prices.csv
│       ├── iris.csv
│       ├── customer_churn.csv
│       ├── weather.csv
│       └── products.json
├── 08-Machine-Learning/
├── 09-Advanced-Machine-Learning/
├── 10-Deep-Learning-and-AI/
├── 13-Interview-Preparation/
├── 14-Cheatsheets/
└── 15-Resources/
```

Some folders may contain additional nested modules, and other sections may be present in the repository. Check GitHub for the current structure rather than assuming this illustration lists every file.

### Important resource files

- [Learning Guide](LEARNING-GUIDE.md) — study workflow, learning sequence, progress checklist, and project guidance.
- [Roadmap](ROADMAP.md) — topic navigation, if available.
- [Sample Datasets](datasets/sample/README.md) — descriptions and example exercises for the sample data.
- [Data Dictionary](datasets/sample/data_dictionary.md) — column definitions for the sample datasets.
- [Cheatsheets](14-Cheatsheets/) — quick references for revising key concepts.
- [Interview Preparation](13-Interview-Preparation/) — interview topics and practice material.
- [Resources](15-Resources/) — datasets, official documentation, and research references.

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/cubicnova/Data-Science-Roadmap.git
cd Data-Science-Roadmap
```

### 2. Create a virtual environment

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

If PowerShell blocks activation, review your local execution-policy settings or use another supported environment activation method. Do not change system security settings without understanding the consequences.

### 3. Install the basic data science stack

```bash
python -m pip install numpy pandas matplotlib seaborn scikit-learn jupyter
```

These packages cover many introductory exercises. Install additional libraries only when needed by a module. For deep learning frameworks, GPU support, or specialized tools, follow their current official installation instructions.

### 4. Launch Jupyter

```bash
jupyter notebook
```

You can also use JupyterLab or Visual Studio Code with its Python and Jupyter extensions.

### 5. Verify your setup

```python
import sys
import numpy
import pandas
import matplotlib
import sklearn

print("Python:", sys.version)
print("NumPy:", numpy.__version__)
print("Pandas:", pandas.__version__)
print("Matplotlib:", matplotlib.__version__)
print("Scikit-learn:", sklearn.__version__)
```

If a module requires a different package version, follow that module's instructions. Consider using a dedicated virtual environment for separate projects.

## Sample Datasets

The `datasets/sample/` directory provides small datasets for practicing Python, Pandas, visualization, statistics, and introductory machine learning.

| File | Practice area |
|---|---|
| [`students.csv`](datasets/sample/students.csv) | Attendance, study hours, exam scores, grouping, and visualization |
| [`sales.csv`](datasets/sample/sales.csv) | Transactions, discounts, revenue, and date-based aggregation |
| [`employees.csv`](datasets/sample/employees.csv) | Department-level summaries and salary distributions |
| [`house_prices.csv`](datasets/sample/house_prices.csv) | Regression and feature engineering |
| [`iris.csv`](datasets/sample/iris.csv) | Classification and model evaluation |
| [`customer_churn.csv`](datasets/sample/customer_churn.csv) | Classification, churn analysis, and precision-recall trade-offs |
| [`weather.csv`](datasets/sample/weather.csv) | Date handling, trends, and time-series exploration |
| [`products.json`](datasets/sample/products.json) | JSON parsing and structured-data transformations |

See the [sample dataset README](datasets/sample/README.md) and [data dictionary](datasets/sample/data_dictionary.md) for details.

**Dataset note:** The sample datasets are for educational practice. Most are synthetic and should not be treated as evidence of real-world behavior. The Iris dataset is a classic public dataset; consult its documentation for provenance and licensing. Check source and license terms before redistributing any third-party data.

### Example: Load and inspect a dataset

Run this from the repository root:

```python
import pandas as pd

df = pd.read_csv("datasets/sample/sales.csv")

print(df.head())
print("Shape:", df.shape)
print(df.info())
print(df.isna().sum())
```

Use the actual file path and column names from the dataset documentation if your local structure differs.

## Recommended Learning Workflow

For each topic:

1. Read the explanation and learning objectives.
2. Run the code examples.
3. Change inputs and parameters to understand the behavior.
4. Complete exercises independently.
5. Review solutions only after making a genuine attempt, where solutions are available.
6. Write a short summary in your own words.
7. Apply the concept to a dataset or mini-project.
8. Record mistakes, limitations, and questions to revisit.

### Build a portfolio gradually

Start with a small EDA project, then progress to a baseline machine learning project, and later build a more complete workflow. A strong project should explain the problem, data source, cleaning decisions, method, evaluation, limitations, and how to reproduce the results.

Do not report only a model score. Explain how it was measured, what the baseline achieved, and where the approach may fail.

## Project-Based Learning

Potential projects include:

- Student performance exploratory analysis.
- Sales analysis and reporting.
- House-price regression.
- Customer-churn classification.
- Customer segmentation.
- Time-series forecasting.
- Anomaly detection.
- Image classification.
- Text classification or sentiment analysis.
- A recommendation system.
- A small RAG evaluation project.
- An end-to-end model deployment and monitoring workflow.

Choose a project that matches the topics you have studied. Begin with a simple baseline, define suitable evaluation metrics, and add complexity only when it provides value.

## Research and Interview Preparation

Use the relevant folders when you are ready to deepen your understanding:

- [`13-Interview-Preparation/`](13-Interview-Preparation/) — technical revision, case studies, and coding challenges.
- [`14-Cheatsheets/`](14-Cheatsheets/) — quick-reference notes for key tools and concepts.
- [`15-Resources/`](15-Resources/) — dataset sources, official documentation, and research papers.

For research-based work, read the original paper where possible, record the method and assumptions, check the experimental setup, and document any differences between published and reproduced results.

For interviews, practice explaining concepts, writing code without copying examples, choosing evaluation metrics, identifying data leakage, and communicating trade-offs clearly.

## Learning Progress Checklist

Use this checklist as a starting point. Mark items complete when you can perform them independently, not simply when you have read the topic.

- [ ] Set up Python, a virtual environment, Git, and Jupyter.
- [ ] Write Python functions and work with files and collections.
- [ ] Explain core statistical concepts and summary measures.
- [ ] Load, clean, transform, and analyze tabular data.
- [ ] Use SQL for common filtering, joins, and aggregations.
- [ ] Create meaningful visualizations and explain findings.
- [ ] Train and evaluate a baseline machine learning model.
- [ ] Avoid data leakage and use suitable validation.
- [ ] Complete and document at least one end-to-end project.
- [ ] Explain project decisions, limitations, and future improvements.
- [ ] Explore one specialization relevant to your goals.
- [ ] Read and summarize a research paper or technical report.

## Recommended Tools

| Purpose | Tools to explore |
|---|---|
| Programming | Python, Jupyter, VS Code |
| Version control | Git, GitHub |
| Data analysis | NumPy, Pandas |
| Visualization | Matplotlib, Seaborn |
| Machine learning | Scikit-learn, XGBoost, LightGBM |
| Deep learning | PyTorch or TensorFlow |
| NLP and language models | Hugging Face Transformers |
| Databases | SQL, SQLite, PostgreSQL |
| Experiment tracking | MLflow or comparable tools |
| Testing and code quality | pytest, Ruff, formatting and linting tools |

You do not need to install every tool at once. Start with Python, NumPy, Pandas, Matplotlib, and Scikit-learn, then add tools as your projects require them.

## Learning Resources

Prefer official documentation when checking APIs, supported versions, and implementation details.

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

## Contributing

Contributions that improve clarity, correctness, and practical value are welcome.

You can help by:

- Fixing typos or broken links.
- Improving explanations.
- Adding tested code examples.
- Creating practice questions with clear solutions.
- Adding suitable datasets with source and license information.
- Improving project instructions.
- Updating outdated references.

Before submitting changes:

- [ ] Confirm that links and relative file paths work.
- [ ] Test code examples where practical.
- [ ] Cite original sources when appropriate.
- [ ] Document dataset provenance and license.
- [ ] Avoid committing secrets, private information, or restricted datasets.
- [ ] Keep changes focused and consistent with the existing structure.

### Suggested contribution workflow

```bash
git checkout -b docs/improve-readme
# Make and review your changes
git add README.md
git commit -m "docs: improve data science roadmap README"
git push origin docs/improve-readme
```

Then open a pull request on GitHub if you use a fork or branch-based workflow.

## License

Review the repository's [`LICENSE`](LICENSE) file for the applicable license and terms of use. Respect the separate licenses and terms of any third-party datasets, code, images, or documentation referenced in this repository.

---

Maintained by **CubicNova**.

If this roadmap helps you learn, consider starring the repository, sharing constructive feedback, or contributing an improvement.
