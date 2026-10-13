# 06 - Git and GitHub

Git and GitHub are essential tools for **managing, tracking, sharing, and collaborating** on data science projects. They help you keep a history of your code, organize notebooks, document experiments, and publish projects as a professional portfolio.

Although they are used together, **Git and GitHub are different tools**.

| Tool | What it is |
|------|------------|
| **[Git](https://git-scm.com/)** | A distributed version control system that tracks changes to files, supports branches, compares revisions, and restores earlier versions |
| **[GitHub](https://github.com/)** | An online platform for hosting Git repositories, with pull requests, issues, code reviews, documentation, and automated workflows |

> ⏱️ **Estimated time:** 3-4 hours  
> 📌 **Prerequisites:** [05 - Jupyter Notebook](../05-Jupyter-Notebook/)  
> 🎯 **Level:** Beginner

---

## 📖 What You'll Learn

- [ ] Explain the difference between Git and GitHub
- [ ] Understand key terms: repository, commit, branch, push, pull, and more
- [ ] Install and configure Git
- [ ] Use the essential Git commands
- [ ] Upload a data science project to GitHub
- [ ] Structure a repository so others can reproduce your work
- [ ] Avoid committing sensitive data

---

## 1. Why Git and GitHub Matter in Data Science

| Benefit | Description |
|---------|-------------|
| **Version control** | Track changes to scripts, notebooks, and documentation |
| **Experiment management** | Keep the code versions used for different analyses and models |
| **Collaboration** | Work with other data scientists on shared projects |
| **Portfolio development** | Showcase projects to employers and the community |
| **Reproducibility** | Document the code and dependencies needed to repeat an analysis |
| **Project recovery** | Restore earlier versions when a change introduces an error |

**Good portfolio projects:** sales analysis, customer churn prediction, data visualization dashboards, and machine learning models.

---

## 2. Key Terminology

| Term | Meaning |
|------|---------|
| **Repository** | A project folder with files and version history |
| **Commit** | A saved snapshot of changes |
| **Branch** | An independent line of development |
| **Clone** | A local copy of a remote repository and its history |
| **Push** | Upload local commits to a remote repository |
| **Pull** | Fetch and integrate changes from a remote repository |
| **Fetch** | Download remote updates without integrating them |
| **Merge** | Combine changes from different branches |
| **Pull request** | Propose changes for review and merging |
| **`.gitignore`** | Lists files Git should normally ignore |

---

## 3. Installing Git

Download Git from [git-scm.com/downloads](https://git-scm.com/downloads). After installation, open PowerShell, Command Prompt, or a terminal in VS Code.

Verify the installation:

```bash
git --version
```

Configure your commit identity (use the name and email you want on your commits):

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

---

## 4. Essential Git Commands

| Command | Purpose |
|---------|---------|
| `git init` | Initialize a local repository |
| `git clone URL` | Clone an existing repository |
| `git status` | Check file and repository status |
| `git add .` | Stage changes in the current directory |
| `git commit -m "message"` | Create a commit |
| `git log --oneline` | View concise commit history |
| `git branch` | List branches |
| `git switch -c feature-name` | Create and switch to a branch |
| `git pull` | Fetch and integrate remote changes |
| `git push` | Push commits to a remote repository |
| `git diff` | View unstaged changes |

---

## 5. Practical: Upload a Project to GitHub

Suppose you have a project named `student-performance-analysis`.

### Step 1: Create the project folder

```bash
mkdir student-performance-analysis
cd student-performance-analysis
```

Create a `README.md` with your project description (use your editor, or the command below). You can also add notebooks and datasets to this folder.

```bash
echo "# Student Performance Analysis" > README.md
```

### Step 2: Initialize Git

```bash
git init
```

### Step 3: Stage and commit files

```bash
git add .
git commit -m "Initial Data Science project"
```

### Step 4: Create a GitHub repository

1. Open [GitHub](https://github.com/)
2. Create a new repository named `student-performance-analysis`
3. If you already have local files, leave **Add a README**, **.gitignore**, and **license** unchecked, to avoid an initial-history conflict
4. Copy the repository URL

### Step 5: Connect and push

Replace the URL with your own:

```bash
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/student-performance-analysis.git
git push -u origin main
```

Your committed files should now appear on GitHub. For the official walkthrough, see the [GitHub Hello World guide](https://docs.github.com/en/get-started/quickstart/hello-world).

---

## 6. Recommended Repository Structure

Organize your repository so others can understand and reproduce your work.

```
student-performance-analysis/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   └── README.md
├── notebooks/
│   └── student_analysis.ipynb
├── src/
│   └── analysis.py
├── reports/
│   └── findings.md
└── images/
    └── performance_chart.png
```

| Item | Purpose |
|------|---------|
| `README.md` | Project overview, setup, methodology, and results |
| `requirements.txt` | Python package dependencies |
| `data/` | Dataset location, or instructions for obtaining the data |
| `notebooks/` | Jupyter notebooks for experiments and analysis |
| `src/` | Reusable Python scripts |
| `reports/` | Findings and conclusions |
| `images/` | Charts and supporting visuals |

A starter `.gitignore` for Python and Jupyter projects:

```
__pycache__/
*.pyc
.ipynb_checkpoints/
.venv/
.env
.DS_Store
```

> ⚠️ **Never commit** passwords, API keys, confidential datasets, or personally identifiable information. Notebook outputs can also contain sensitive information, so review them before pushing.

---

## 7. Git Workflow for Data Science

```
Create or edit files   →  scripts, notebooks, documentation
        ↓
Check changes          →  git status, git diff
        ↓
Stage changes          →  git add
        ↓
Commit changes         →  git commit
        ↓
Push to GitHub         →  git push
        ↓
Review & collaborate   →  pull requests and code review
```

---

## ✅ Practice Checklist

- [ ] Check the installed Git version
- [ ] Configure your Git username and email
- [ ] Create a local repository for a Python project
- [ ] Create a `README.md` file
- [ ] Stage and commit your first project files
- [ ] Create a GitHub repository and push your project
- [ ] Modify the README and push another commit
- [ ] Create a feature branch and merge a change
- [ ] Create a `.gitignore` file for Python and Jupyter projects

---

## ✅ Self-Check Questions

- What is the difference between Git and GitHub?
- What is the difference between `git fetch` and `git pull`?
- Why should you check `git status` before committing?
- Why are notebook outputs a privacy risk?

---

## 📚 Learning Resources

- [Git official documentation](https://git-scm.com/doc)
- [Getting started with GitHub](https://docs.github.com/en/get-started)
- [GitHub Skills: interactive courses](https://skills.github.com/)

---

## ➡️ Next Up

You've finished **Getting Started**! 🎉 Continue to [01 - Python Programming](../../01-Python-Programming/). When you're ready for more, revisit Git branching, merging, and resolving merge conflicts, then build a professional data science portfolio on GitHub.

---

[⬅️ Previous: Jupyter Notebook](../05-Jupyter-Notebook/) · [⬅️ Back to Getting Started](../README.md) · [🏠 Main Roadmap](../../README.md)
