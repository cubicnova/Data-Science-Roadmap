# 03 - Tools and Environment

Data science tools and environments are the **languages, libraries, editors, and platforms** used to collect, process, analyze, visualize, and model data. The right setup helps you work efficiently, collaborate with others, and keep your results reproducible.

> ⏱️ **Estimated time:** 3-4 hours  
> 📌 **Prerequisites:** [02 - Data Science Lifecycle](../02-Data-Science-Lifecycle/)  
> 🎯 **Level:** Beginner

---

## 📖 What You'll Learn

- [ ] Identify the main categories of data science tools
- [ ] Choose a suitable development environment for your needs
- [ ] Describe the role of the essential Python libraries
- [ ] Set up a working environment with Google Colab or local Python
- [ ] Run a first analysis with Pandas and Matplotlib
- [ ] Explain why version control and reproducibility matter

---

## 1. Categories of Data Science Tools

| Category | Purpose | Examples |
|----------|---------|----------|
| **Programming languages** | Process data, automate tasks, build models | Python, R, SQL, Julia |
| **Development environments** | Write and run code, debug, document experiments | Jupyter Notebook, JupyterLab, VS Code, PyCharm, Google Colab |
| **Data processing libraries** | Numerical computing, manipulation, preprocessing | NumPy, Pandas, Polars, SciPy |
| **Visualization tools** | Charts, graphs, dashboards, reports | Matplotlib, Seaborn, Plotly, Power BI, Tableau |
| **Machine learning frameworks** | Train, evaluate, and implement models | Scikit-learn, TensorFlow, PyTorch, XGBoost |
| **Database systems** | Store, organize, and query data | MySQL, PostgreSQL, SQLite, MongoDB |

---

## 2. Common Data Science Environments

An **environment** is the setup in which your programs are developed and run: the operating system, programming language, libraries, dependencies, and computing resources.

| Environment | Description | Best use |
|-------------|-------------|----------|
| **Jupyter Notebook** | Interactive coding with executable cells | Learning and exploratory analysis |
| **Google Colab** | Cloud-based notebooks | Beginners and GPU-supported experiments |
| **VS Code** | Extensible code editor | Projects and application development |
| **PyCharm** | Python-focused IDE | Larger Python applications |
| **Anaconda** | Python distribution with environment management | Managing scientific Python packages |
| **Cloud platforms** | Remote computing, storage, and managed services | Large-scale or collaborative workloads |

---

## 3. Essential Python Libraries

| Library | Purpose | Area |
|---------|---------|------|
| **NumPy** | Arrays, matrices, and mathematical operations | Numerical computing |
| **Pandas** | Filtering, aggregation, and cleaning of tabular data | Data analysis |
| **Matplotlib / Seaborn** | Statistical charts and visual exploration | Visualization |
| **Scikit-learn** | Classification, regression, clustering, preprocessing, evaluation | Machine learning |

---

## 4. Setting Up Your Environment

Python and Jupyter Notebook are a practical starting point.

### Option A: Google Colab (no installation)

1. Open [Google Colab](https://colab.research.google.com/)
2. Create a new notebook
3. Type Python code into a cell
4. Run the cell to see the output
5. Import datasets and start practicing

### Option B: Python locally

Install Python from [python.org](https://www.python.org/), then create a virtual environment:

```bash
python -m venv ds-env
```

Activate it:

```bash
# Windows
ds-env\Scripts\activate

# macOS / Linux
source ds-env/bin/activate
```

Install the basic libraries:

```bash
python -m pip install numpy pandas matplotlib seaborn scikit-learn jupyter
```

Launch Jupyter Notebook:

```bash
jupyter notebook
```

> 💡 A **virtual environment** keeps each project's dependencies separate from your other Python projects.

---

## 5. Your First Analysis

This example uses Pandas to analyze student marks and Matplotlib to chart them.

```python
import pandas as pd
import matplotlib.pyplot as plt

data = {
    "Student": ["Amit", "Priya", "Rahul", "Neha"],
    "Marks": [75, 90, 65, 85]
}

df = pd.DataFrame(data)

print(df)
print("Average marks:", df["Marks"].mean())

df.plot(
    x="Student",
    y="Marks",
    kind="bar",
    legend=False,
    title="Student Performance"
)
plt.ylabel("Marks")
plt.tight_layout()
plt.show()
```

**What this demonstrates:**
- Pandas organizes the data in a table
- `mean()` calculates the average marks
- Matplotlib generates a bar chart
- The chart makes it easy to compare student performance

---

## 6. Version Control and Collaboration

Projects often involve many scripts, datasets, notebooks, and model versions. These tools help you track changes and work with others.

| Tool | Purpose |
|------|---------|
| **Git** | Tracks changes to project files |
| **GitHub** | Hosts repositories and supports collaboration |
| **Docker** | Packages applications and dependencies into containers |
| **DVC** | Versions large datasets and ML artifacts |
| **MLflow** | Tracks experiments and manages the model lifecycle |

They become more valuable as projects move from experimentation to production.

---

## 7. Recommended Learning Sequence

1. Python and Jupyter Notebook
2. NumPy and Pandas
3. SQL and database fundamentals
4. Matplotlib and Seaborn
5. Scikit-learn
6. Git and GitHub
7. Streamlit or FastAPI for deployment
8. Docker, experiment tracking, and cloud platforms

> 💡 **Key takeaway:** You do not need every tool on day one. Start with **Python, Jupyter Notebook, Pandas, NumPy, and Matplotlib**, and add machine learning and deployment tools as your skills and projects grow.

---

## 📝 Practice

1. Open Google Colab and run a cell that prints "Hello, Data Science!"
2. Set up a local virtual environment and install the basic libraries.
3. Run the student marks example, then add two more students and a new chart.
4. Create a Jupyter notebook with a markdown title and a code cell, and save it.
5. Pick one tool from each category in Section 1 and write one line on what it does.
6. Create a GitHub account and a test repository, then commit a notebook to it.

---

## ✅ Self-Check Questions

- When would you choose Google Colab over a local Jupyter setup?
- Why use a virtual environment for each project?
- What is the difference between NumPy and Pandas?
- Which tools help reproducibility, and why?

---

## 📚 Further Reading

- [Python documentation](https://docs.python.org/3/)
- [Jupyter documentation](https://jupyter.org/documentation)
- [Pandas user guide](https://pandas.pydata.org/docs/user_guide/index.html)
- [Git documentation](https://git-scm.com/doc)

---

## ➡️ Next Up

[04 - Python Installation](../04-Python-Installation/): install Python step by step and verify your setup.

---

[⬅️ Previous: Data Science Lifecycle](../02-Data-Science-Lifecycle/) · [⬅️ Back to Getting Started](../README.md) · [🏠 Main Roadmap](../../README.md)
