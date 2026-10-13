# 05 - Jupyter Notebook

Jupyter Notebook is an open-source, interactive computing environment where you can **write and run code, display outputs, create visualizations, and document your work in a single file**. It is widely used in data science, scientific computing, machine learning, and teaching.

Jupyter supports multiple programming languages through **kernels**. Python is the most common.

> ⏱️ **Estimated time:** 2-3 hours  
> 📌 **Prerequisites:** [04 - Python Installation](../04-Python-Installation/)  
> 🎯 **Level:** Beginner

---

## 📖 What You'll Learn

- [ ] Explain what Jupyter Notebook is and why it is used
- [ ] Install and launch Jupyter Notebook
- [ ] Navigate the interface and understand kernels
- [ ] Work with code cells and Markdown cells
- [ ] Use essential keyboard shortcuts
- [ ] Run a small data analysis with Pandas and Matplotlib
- [ ] Save, export, and organize notebooks in a project

---

## 1. Key Features

| Feature | Description |
|---------|-------------|
| **Interactive coding** | Execute code one cell at a time |
| **Immediate output** | Results appear directly below the code cell |
| **Data visualization** | Charts and plots display inline |
| **Markdown support** | Headings, explanations, lists, and math formulas |
| **Data analysis** | Works with Pandas, NumPy, and other libraries |
| **Documentation** | Code, output, and explanations in one document |
| **Reproducibility** | Save notebooks so analysis can be reviewed and rerun |

---

## 2. Installation

Make sure Python is installed first:

```bash
python --version
```

Install Jupyter Notebook:

```bash
python -m pip install notebook
```

Start it:

```bash
jupyter notebook
```

Your default browser should open the Jupyter interface. If it doesn't, check the terminal for the local URL printed by Jupyter.

**Recommended: use an isolated project environment**

```bash
python -m venv ds-env

# Windows
ds-env\Scripts\activate
# macOS / Linux
source ds-env/bin/activate

python -m pip install notebook numpy pandas matplotlib
jupyter notebook
```

---

## 3. The Jupyter Interface

| Component | Description |
|-----------|-------------|
| **Notebook** | The document containing code and text cells |
| **Code cell** | Write and run programming code |
| **Markdown cell** | Add formatted text and documentation |
| **Toolbar** | Shortcuts for running, adding, and managing cells |
| **Kernel** | Executes code and keeps the current session state |
| **Cell output** | Shows results, errors, tables, and visualizations |
| **File browser** | Create, open, and manage notebook files |

---

## 4. Your First Notebook

1. Start Jupyter Notebook from the terminal
2. In the browser, create a new notebook with the **Python kernel**
3. Type the code below into the first cell
4. Run it with **Shift + Enter**

```python
print("Hello, Data Science!")
```

Output:

```
Hello, Data Science!
```

---

## 5. Types of Cells

### A. Code Cell
Executes Python statements.

```python
a = 10
b = 20
result = a + b
print("Sum:", result)
```

Output:

```
Sum: 30
```

### B. Markdown Cell
Holds formatted documentation instead of executable code.

```markdown
# Data Science

## Introduction
Data Science uses data to discover insights.

**Important:** Data cleaning is an essential step.
```

When run, the text renders as formatted headings and paragraphs. To change a cell's type, use the toolbar dropdown or the shortcuts below.

---

## 6. Keyboard Shortcuts

Jupyter has two modes: **Command mode** (press `Esc`) and **Edit mode** (press `Enter`).

| Shortcut | Function |
|----------|----------|
| `Shift + Enter` | Run cell and select the next cell |
| `Ctrl + Enter` | Run cell and stay on it |
| `Alt + Enter` | Run cell and insert a new cell below |
| `Esc` | Enter Command mode |
| `Enter` | Enter Edit mode |
| `A` | Insert cell above (Command mode) |
| `B` | Insert cell below (Command mode) |
| `M` | Convert cell to Markdown (Command mode) |
| `Y` | Convert cell to Code (Command mode) |
| `D`, `D` | Delete selected cell (Command mode) |

> These are the classic Notebook shortcuts. Some may differ in newer Jupyter interfaces or with customized settings.

---

## 7. Data Analysis in Jupyter

Install the libraries from inside the notebook if needed:

```python
%pip install pandas numpy matplotlib
```

Then create and analyze a dataset:

```python
import pandas as pd
import matplotlib.pyplot as plt

data = {
    "Student": ["Amit", "Priya", "Rahul", "Neha"],
    "Marks": [75, 90, 65, 85]
}

df = pd.DataFrame(data)

display(df)
print("Average marks:", df["Marks"].mean())
print("Highest marks:", df["Marks"].max())

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

**This demonstrates how to:**
- Create a dataset with Pandas
- Display records in a table
- Calculate summary statistics
- Visualize data with Matplotlib

---

## 8. Saving and Exporting

Notebooks use the `.ipynb` extension, for example `data_analysis.ipynb`. Save with **Ctrl + S**.

A notebook file stores code, Markdown text, and saved cell outputs. You can also export to formats such as HTML or PDF, depending on installed tools and configuration.

**Suggested project structure for GitHub:**

```
data-science-project/
├── README.md
├── notebooks/
│   └── exploratory_data_analysis.ipynb
├── data/
│   └── sample_data.csv
├── src/
│   └── analysis.py
└── requirements.txt
```

> ⚠️ Avoid uploading confidential datasets, credentials, or sensitive notebook outputs to a public repository.

---

## 9. Jupyter vs. Other Tools

| Feature | Jupyter Notebook | VS Code | Google Colab |
|---------|------------------|---------|--------------|
| **Execution** | Interactive cells | Scripts and notebooks | Interactive cloud notebooks |
| **Installation** | Usually local | Local application | Browser-based |
| **Internet required** | Not always | Not always | Usually |
| **Local file access** | Yes | Yes | Requires upload or mounting |
| **GPU access** | Depends on local hardware | Depends on configured hardware | Available with plan and resource limits |
| **Best use** | Learning and exploration | Development and project management | Quick experiments and cloud learning |

---

## 10. Advantages and Limitations

| ✅ Advantages | ⚠️ Limitations |
|---------------|----------------|
| Beginner-friendly interactive workflow | Large notebooks become hard to maintain |
| Combines code, explanations, and visuals | Cells run out of order can give confusing results |
| Excellent for teaching and experimentation | Not always the best format for production apps |
| Supports many data science libraries | Large datasets and heavy models may need more resources |

> 💡 **Tip:** Before sharing a notebook, restart the kernel and run all cells from top to bottom to make sure the results are reproducible.

---

## 📝 Practice

Complete these tasks in a new notebook:

1. Print your name and a message about data science.
2. Create two variables and calculate their sum, difference, product, and quotient.
3. Create a list of five numbers and calculate its average.
4. Create a Pandas DataFrame with five student records.
5. Calculate the mean, maximum, and minimum marks.
6. Create a bar chart of the student marks.
7. Add Markdown cells explaining your findings.
8. Save the notebook as `data_science_basics.ipynb`.

---

## ✅ Self-Check Questions

- What is a kernel, and what happens when you restart it?
- When would you use a Markdown cell instead of a code cell?
- What is the difference between `Shift + Enter` and `Ctrl + Enter`?
- Why can running cells out of order cause problems?

---

## 📚 Further Reading

- [Jupyter documentation](https://jupyter.org/documentation)
- [Jupyter Notebook user guide](https://jupyter-notebook.readthedocs.io/)
- [Markdown guide](https://www.markdownguide.org/)

---

## ➡️ Next Up

[06 - Git and GitHub](../06-Git-and-GitHub/): track your work and share it with others. After that, move on to [NumPy](../../03-NumPy/) once you've finished the Python and math modules.

---

[⬅️ Previous: Python Installation](../04-Python-Installation/) · [⬅️ Back to Getting Started](../README.md) · [🏠 Main Roadmap](../../README.md)
