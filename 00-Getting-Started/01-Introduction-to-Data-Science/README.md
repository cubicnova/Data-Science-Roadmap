# 🧭 Data Science Roadmap

A structured, open-source learning path that takes you from **data fundamentals** to **machine learning, deployment, and interview preparation**. Each folder is a self-contained module with notes, exercises, and learning outcomes.

> 🎯 **Goal:** Build the skills needed for a job-ready data science profile, one module at a time.

---

## 📑 Table of Contents

- [Who This Is For](#-who-this-is-for)
- [How to Use This Roadmap](#-how-to-use-this-roadmap)
- [Learning Path at a Glance](#-learning-path-at-a-glance)
- [What You'll Learn](#-what-youll-learn)
- [Tools and Technologies](#-tools-and-technologies)
- [Repository Structure](#-repository-structure)
- [Getting Started](#-getting-started)
- [Contributing](#-contributing)
- [License](#-license)

---

## 👥 Who This Is For

- Complete beginners starting a career in data science
- Students and professionals switching into data roles
- Self-learners who want one organized path instead of scattered tutorials
- Anyone preparing for data analyst, data scientist, or ML interviews

**Prerequisites:** none. Basic computer skills and curiosity are enough.

---

## 🚀 How to Use This Roadmap

1. Follow the folders **in numerical order** (`00` → `15`).
2. In each module, read the `README.md`, then complete the **practical exercises**.
3. Tick off the **learning outcomes** before moving on.
4. Build a project after each phase to lock in the skills.
5. Use [LEARNING-GUIDE.md](LEARNING-GUIDE.md) for a suggested weekly pace and [ROADMAP.md](ROADMAP.md) to track progress.

---

## 🗺️ Learning Path at a Glance

```
Fundamentals → Python → Math & Stats → NumPy/Pandas/SQL → Visualization & EDA
      → Machine Learning → Advanced ML → Deep Learning → Deployment & MLOps
                                  → Projects → Interview Prep
```

| Phase | Folder | Focus |
|-------|--------|-------|
| 🌱 **Foundations** | [`00-Getting-Started`](./00-Getting-Started/) | Data concepts, lifecycle, tools, setup |
| 🐍 | [`01-Python-Programming`](./01-Python-Programming/) | Python from basics to OOP |
| 📐 | [`02-Mathematics-and-Statistics`](./02-Mathematics-and-Statistics/) | Linear algebra, probability, inference |
| 🧮 **Data Handling** | [`03-NumPy`](./03-NumPy/) | Arrays and numerical computing |
| 🐼 | [`04-Pandas-and-Data-Wrangling`](./04-Pandas-and-Data-Wrangling/) | Data acquisition, cleaning, preprocessing |
| 🗄️ | [`05-SQL-for-Data-Science`](./05-SQL-for-Data-Science/) | Querying and database design |
| 📊 **Analysis** | [`06-Data-Visualization`](./06-Data-Visualization/) | Matplotlib, Seaborn, Plotly, dashboards |
| 🔍 | [`07-Exploratory-Data-Analysis`](./07-Exploratory-Data-Analysis/) | Univariate to multivariate EDA |
| 🤖 **Machine Learning** | [`08-Machine-Learning`](./08-Machine-Learning/) | Regression, classification, clustering, evaluation |
| ⚡ | [`09-Advanced-Machine-Learning`](./09-Advanced-Machine-Learning/) | Ensembles, boosting, time series |
| 🧠 **Deep Learning** | [`10-Deep-Learning-and-AI`](./10-Deep-Learning-and-AI/) | Neural networks, CV, NLP, transformers |
| 🚢 **Production** | [`11-Deployment-and-MLOps`](./11-Deployment-and-MLOps/) | FastAPI, Docker, CI/CD, monitoring |
| 💼 **Career** | [`12-Real-World-Projects`](./12-Real-World-Projects/) | Beginner to end-to-end projects |
| | [`13-Interview-Preparation`](./13-Interview-Preparation/) | Questions, case studies, coding challenges |
| | [`14-Cheatsheets`](./14-Cheatsheets/) · [`15-Resources`](./15-Resources/) | Quick references, books, courses, datasets |

---

## 📚 What You'll Learn

### 1. Data Fundamentals and Classification
Understand what data is and how it is organized.
- Data vs. information vs. knowledge
- Data quality: accuracy, completeness, consistency, validity, timeliness, uniqueness
- Classification by **structure** (structured, semi-structured, unstructured), **nature** (qualitative, quantitative), **measurement** (discrete, continuous), **scale** (nominal, ordinal, interval, ratio), and **source** (primary, secondary)
- Data sensitivity levels and common file formats (CSV, JSON, XML, Parquet)

### 2. Data Science Lifecycle and Methodologies
Learn how real projects are run from idea to production.
- **8 phases:** Business Understanding → Data Collection → Data Preparation → EDA → Model Development → Evaluation → Deployment → Monitoring
- **Methodologies:** CRISP-DM, KDD, OSEMN, Agile Data Science, TDSP
- Case study: customer churn prediction

### 3. Mathematical and Statistical Foundations
The theory behind every model.
- **Math:** basic mathematics, linear algebra, calculus (gradients, optimization), discrete mathematics
- **Statistics:** descriptive statistics, probability, distributions, sampling, estimation, hypothesis testing, correlation, regression

### 4. Data Acquisition, Cleaning, and Preprocessing
Turn raw data into analysis-ready data.
- **Acquisition:** files, databases, APIs, permitted web scraping, streaming
- **Cleaning:** missing values, duplicates, outliers (IQR, z-score), inconsistent formats, validation
- **Preprocessing:** integration, transformation, scaling, encoding, feature engineering, imbalanced data, train/validation/test splitting without **data leakage**

### 5. Exploratory Data Analysis and Visualization
Discover patterns before modeling.
- Dataset profiling and data-quality checks
- Univariate, bivariate, and multivariate analysis
- Choosing the right chart: histogram, box plot, scatter, heatmap, pair plot, and more
- Time-series EDA: trends, seasonality, rolling averages
- Communicating findings clearly and honestly

### 6. Introduction to Machine Learning
Build and evaluate your first models.
- **Types:** supervised, unsupervised, semi-supervised, reinforcement learning
- **Supervised:** Linear/Logistic Regression, Decision Trees, Random Forest, KNN, SVM, Naive Bayes
- **Unsupervised:** K-Means, hierarchical clustering, DBSCAN, PCA, association rules
- **Evaluation:** accuracy, precision, recall, F1, ROC-AUC, MAE, RMSE, R²
- Overfitting, the bias-variance trade-off, cross-validation, and pipelines

### 7. Data Science Tools and Technologies
Pick the right tool for each stage of the lifecycle. See the table below.

---

## 🛠️ Tools and Technologies

| Category | Tools |
|----------|-------|
| **Languages** | Python, R, SQL |
| **Data manipulation** | NumPy, Pandas, SciPy, Apache Spark |
| **Notebooks & IDEs** | Jupyter, JupyterLab, Google Colab, VS Code, PyCharm, RStudio |
| **Visualization & BI** | Matplotlib, Seaborn, Plotly, Altair, Power BI, Tableau |
| **Databases** | PostgreSQL, MySQL, SQLite, MongoDB, Snowflake, BigQuery |
| **ML / DL** | Scikit-learn, XGBoost, LightGBM, TensorFlow, Keras, PyTorch |
| **Big data & pipelines** | Hadoop, Kafka, Airflow, dbt, Databricks |
| **Cloud** | AWS, Microsoft Azure, Google Cloud |
| **MLOps & collaboration** | Git, GitHub, DVC, MLflow, Docker, FastAPI, GitHub Actions |

---

## 📂 Repository Structure

```
Data-Science-Roadmap/
├── 00-Getting-Started/            ├── 08-Machine-Learning/
├── 01-Python-Programming/         ├── 09-Advanced-Machine-Learning/
├── 02-Mathematics-and-Statistics/ ├── 10-Deep-Learning-and-AI/
├── 03-NumPy/                      ├── 11-Deployment-and-MLOps/
├── 04-Pandas-and-Data-Wrangling/  ├── 12-Real-World-Projects/
├── 05-SQL-for-Data-Science/       ├── 13-Interview-Preparation/
├── 06-Data-Visualization/         ├── 14-Cheatsheets/
├── 07-Exploratory-Data-Analysis/  ├── 15-Resources/
├── datasets/   assets/   tests/   └── .github/
└── README.md  ROADMAP.md  LEARNING-GUIDE.md  CONTRIBUTING.md  LICENSE
```

Each topic folder contains a `README.md` with objectives, notes, practice exercises, and resources.

---

## ⚡ Getting Started

```bash
# 1. Clone the repository
git clone https://github.com/cubicnova/Data-Science-Roadmap.git
cd Data-Science-Roadmap

# 2. Create a virtual environment
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Launch Jupyter
jupyter notebook
```

Then open [`00-Getting-Started`](./00-Getting-Started/) and begin with **Introduction to Data Science**.

---

## ✅ Learning Outcomes

By completing this roadmap you will be able to:

- Explain data concepts, classifications, and quality dimensions
- Run a project through the full data science lifecycle
- Apply core math, probability, and statistics to real problems
- Acquire, clean, and preprocess data without leakage
- Perform EDA and communicate insights with effective visualizations
- Build, evaluate, and tune machine learning models
- Use version control, SQL, cloud platforms, and MLOps tools
- Present a portfolio of end-to-end projects

---

## 🤝 Contributing

Contributions are welcome: fix typos, add exercises, improve explanations, or suggest resources.

1. Fork the repository
2. Create a branch: `git checkout -b feature/your-topic`
3. Commit your changes and open a Pull Request

Please read [CONTRIBUTING.md](CONTRIBUTING.md) and [CODE-OF-CONDUCT.md](CODE-OF-CONDUCT.md) first.

---

## 📄 License

Released under the [MIT License](LICENSE).

---

⭐ If this roadmap helps you, consider giving the repo a star and sharing it with others.
