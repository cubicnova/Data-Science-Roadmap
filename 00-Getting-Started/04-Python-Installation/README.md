# 04 - Python Installation

Python is a high-level, general-purpose language widely used in data science, machine learning, AI, and automation. Before you can use libraries like NumPy, Pandas, Matplotlib, and Scikit-learn, you need a working Python installation (or a cloud-based Python environment).

> ⏱️ **Estimated time:** 1-2 hours  
> 📌 **Prerequisites:** [03 - Tools and Environment](../03-Tools-and-Environment/)  
> 🎯 **Level:** Beginner

---

## 📖 What You'll Learn

- [ ] Check that your computer meets the system requirements
- [ ] Download and install Python from the official website
- [ ] Verify Python and pip from the command line
- [ ] Run your first Python program
- [ ] Install the essential data science libraries
- [ ] Launch Jupyter Notebook and run a first analysis

---

## 1. System Requirements

| Component | Recommendation |
|-----------|----------------|
| **Operating system** | Windows 10/11, Linux, or macOS |
| **Processor** | Modern dual-core or better |
| **RAM** | 4 GB minimum; 8 GB or more recommended |
| **Storage** | 2-5 GB free for a basic setup and packages |
| **Internet** | Required for downloading Python and packages |

> These are practical recommendations, not strict minimums.

> 💡 **No installation option:** If you'd rather skip setup for now, use [Google Colab](https://colab.research.google.com/) in your browser and come back to this later.

---

## 2. Download Python

Download a current stable **Python 3** release for your operating system from the official website:

👉 [python.org/downloads](https://www.python.org/downloads/)

---

## 3. Installation on Windows

**Step 1: Download the installer**  
Open the official downloads page and download the Windows installer for a supported Python 3 version.

**Step 2: Run the installer**  
Open the downloaded file. If the option is available, tick **Add python.exe to PATH**, then choose the install option.

**Step 3: Complete the installation**  
Wait for setup to finish, then close the installer.

<details>
<summary><b>macOS / Linux</b></summary>

- **macOS:** Install the package from python.org, or use Homebrew: `brew install python`
- **Linux (Debian/Ubuntu):** Python 3 is usually preinstalled. If not: `sudo apt install python3 python3-pip python3-venv`

On macOS and Linux the commands are typically `python3` and `pip3` instead of `python` and `pip`.

</details>

---

## 4. Verify the Installation

Open **Command Prompt** or **PowerShell** and run:

```bash
python --version
```

If that doesn't work, try:

```bash
py --version
```

Expected output (the version depends on what you installed):

```
Python 3.x.x
```

Then confirm that pip, Python's package manager, is available:

```bash
python -m pip --version
```

> ⚠️ **Troubleshooting:** If Python is installed but `python` is not recognized, try the Python Launcher (`py`) or check your **PATH** configuration. Re-running the installer with **Add python.exe to PATH** ticked usually fixes it.

---

## 5. Run Your First Python Program

**Interactive mode.** Type `python` in your terminal, then at the prompt enter:

```python
print("Hello, Data Science!")
```

Expected output:

```
Hello, Data Science!
```

Type `exit()` to leave the interpreter.

**Script mode.** Save this code in a file named `hello.py`:

```python
print("Hello, Data Science!")
print("Python installation successful.")
```

Run it from the folder containing the file:

```bash
python hello.py
```

---

## 6. Install Essential Data Science Libraries

```bash
python -m pip install numpy pandas matplotlib seaborn scikit-learn jupyter
```

| Library | Purpose |
|---------|---------|
| **NumPy** | Numerical calculations and arrays |
| **Pandas** | Data manipulation and analysis |
| **Matplotlib** | Charts and graphs |
| **Seaborn** | Statistical visualization |
| **Scikit-learn** | Machine learning |
| **Jupyter** | Interactive coding and experimentation |

Verify that everything installed correctly:

```bash
python -c "import numpy, pandas, matplotlib, seaborn, sklearn; print('All libraries installed successfully')"
```

---

## 7. Launch Jupyter Notebook

```bash
jupyter notebook
```

Your browser should open the Jupyter interface. Create a new Python notebook and run:

```python
import numpy as np
import pandas as pd

data = {"Name": ["Amit", "Priya"], "Marks": [80, 90]}
df = pd.DataFrame(data)

print(df)
print("Average:", np.mean(df["Marks"]))
```

If you see the table and the average (85.0), your environment is ready for basic data analysis. 🎉

---

## 8. Recommended Beginner Setup

| Tool | Purpose |
|------|---------|
| **Python 3** | Programming language |
| **VS Code** | Code editor |
| **Jupyter Notebook** | Interactive learning |
| **Pandas and NumPy** | Data processing |
| **Matplotlib and Seaborn** | Data visualization |
| **Git and GitHub** | Version control |

---

## ✅ Installation Checklist

- [ ] Download Python from the official website
- [ ] Install Python and configure PATH
- [ ] Verify the Python version
- [ ] Verify pip
- [ ] Install data science libraries
- [ ] Launch Jupyter Notebook
- [ ] Run the first Python program

---

## 📝 Practice

1. Print your name and today's goal using a `hello.py` script.
2. Run `python -m pip list` and find the version of each library you installed.
3. Create a notebook and compute the average of five numbers using NumPy.
4. Intentionally mistype a library name in `pip install` and note the error message.
5. Write down the Python version you installed and where it lives on your computer.

---

## ✅ Self-Check Questions

- What does adding Python to PATH allow you to do?
- Why use `python -m pip` instead of just `pip`?
- What is the difference between running Python interactively versus running a `.py` script?
- How can you confirm that all libraries were installed correctly?

---

## 📚 Further Reading

- [Python downloads](https://www.python.org/downloads/)
- [Python tutorial (official)](https://docs.python.org/3/tutorial/)
- [pip user guide](https://pip.pypa.io/en/stable/user_guide/)
- [Jupyter documentation](https://jupyter.org/documentation)

---

## ➡️ Next Up

[05 - Jupyter Notebook](../05-Jupyter-Notebook/): learn to use notebooks effectively. For virtual environments and dependency management, see [Virtual Environments](../../01-Python-Programming/13-Virtual-Environments/).

---

[⬅️ Previous: Tools and Environment](../03-Tools-and-Environment/) · [⬅️ Back to Getting Started](../README.md) · [🏠 Main Roadmap](../../README.md)
