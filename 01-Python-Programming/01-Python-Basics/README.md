# 01 - Python Basics

Welcome to the **Python Basics Practice Workbook** of the Data Science Roadmap. This module builds your Python programming skills, problem-solving ability, and logical thinking through hands-on exercises. They progress from fundamental concepts to intermediate challenges relevant to data science.

> ⏱️ **Estimated time:** 15-20 hours  
> 📌 **Prerequisites:** [Getting Started](../../00-Getting-Started/) (Python 3 installed, Jupyter/Colab/VS Code ready)  
> 🎯 **Level:** Beginner to Intermediate

---

## 📖 Learning Objectives

By completing this module you will be able to:

- [ ] Work with variables, data types, and operators
- [ ] Accept user input and display formatted output
- [ ] Apply conditional statements and loops
- [ ] Manipulate strings and collections
- [ ] Write reusable functions
- [ ] Handle exceptions and work with files
- [ ] Solve problems using basic algorithms
- [ ] Build small Python applications
- [ ] Apply programming fundamentals to simple data analysis tasks

---

## 📋 Prerequisites

- Basic computer knowledge
- Python 3 installed on your system
- Jupyter Notebook, Google Colab, or VS Code
- Familiarity with running a Python program

---

## 🚀 How to Use This Workbook

1. Read the problem statement carefully
2. Identify the required **input, processing, and output**
3. Write your own Python solution
4. Test with normal, boundary, and invalid inputs
5. Add comments and meaningful variable names
6. Compare your output with the expected behavior
7. Save your solutions in a notebook or Python file
8. Commit your completed work to GitHub

> ⚠️ **Important:** Try to solve each exercise independently before looking for hints or solutions.

---

## 🗂️ Workbook Overview

The full exercise statements live in [`practice.md`](./practice.md). Use this table as your map.

| Section | Topic | Exercises | Level |
|---------|-------|-----------|-------|
| **1** | Python Fundamentals: `print()`, comments, variables, data types, user input | 1-5 | Beginner |
| **2** | Operators and Expressions: arithmetic, comparison, logical, assignment, membership | 6-10 | Beginner |
| **3** | Conditional Statements: `if`, `elif`, `else`, nested conditions | 11-16 | Beginner |
| **4** | Loops and Iteration: `for`, `while`, `range()`, `break`, `continue`, nested loops | 17-24 | Beginner to Intermediate |
| **5** | Strings and Collections: strings, lists, tuples, sets, dictionaries, slicing | 25-30 | Beginner to Intermediate |
| **6** | Functions and Modular Programming: parameters, return values, scope | 31-35 | Beginner to Intermediate |
| **7** | Exception Handling and File Operations: `try/except`, text files, CSV | 36-40 | Intermediate |
| **8** | Intermediate Coding Challenges | Challenges 1-7 | Intermediate |
| **9** | Mini-Projects | Projects 1-5 | Intermediate |

### Sample exercises

| # | Exercise | Skills practiced |
|---|----------|------------------|
| 1 | Hello Python | `print()` |
| 5 | Type Conversion | `input()`, `int()`/`float()`, error handling |
| 14 | Grade Calculator | `if/elif/else`, validation |
| 22 | Prime Number Checker | loops, optimization (√n) |
| 29 | Word Frequency Counter | strings, dictionaries |
| 35 | Refactor a Program | functions, `main()`, `if __name__ == "__main__"` |
| 39 | CSV Data Processing | `csv` module, averages |

---

## 🧩 Intermediate Coding Challenges (Section 8)

| # | Challenge | Idea |
|---|-----------|------|
| 1 | Armstrong Number | Digit powers sum to the original number (e.g. 153) |
| 2 | Anagram Checker | Compare two strings after normalizing |
| 3 | Second-Largest Unique Number | Find it without fully sorting the list |
| 4 | Merge Two Dictionaries | Add quantities for repeated products |
| 5 | Two-Sum Problem | Find two indices whose values hit a target |
| 6 | Anagram Groups | Group words that are anagrams of one another |
| 7 | Expense Tracker | CLI app that saves records to CSV |

---

## 🛠️ Mini-Projects (Section 9)

| Project | Objective | Key concepts | Deliverable |
|---------|-----------|--------------|-------------|
| **1. Student Management System** | Add, update, search, and delete student records with grades and averages | dictionaries, functions, loops, exceptions | `student_management.py` |
| **2. Number Guessing Game** | Guess a random number with higher/lower hints and a restart option | `random`, loops, validation | `number_guessing_game.py` |
| **3. Personal Expense Tracker** | Record, view, and summarize expenses saved to CSV | file handling, dates, lists | `expense_tracker.py` |
| **4. Quiz Application** | Multiple-choice Python quiz with scoring and explanations | lists, dictionaries, functions | `python_quiz.py` |
| **5. Basic Data Analysis** | Analyze student performance data (averages, rankings, pass/fail) | sorting, CSV, functions | `student_performance_analysis.py` |

> 💡 **Extension:** Reimplement Project 5 with Pandas in the Pandas module.

---

## 🧪 Testing and Code Quality

For every exercise, check:

- Does the program produce the expected output?
- Does it handle boundary values and invalid input?
- Are variable and function names meaningful?
- Is unnecessary repetition avoided?
- Are comments used where they improve clarity?
- Can the program be run again without unexpected behavior?

Use `assert` statements for simple automated checks:

```python
def add(a, b):
    return a + b

assert add(2, 3) == 5
assert add(-1, 1) == 0
assert add(0, 0) == 0

print("All tests passed!")
```

---

## 📊 Suggested Self-Assessment

| Category | Weight |
|----------|-------:|
| Correctness and expected output | 40% |
| Logic and problem-solving | 25% |
| Code readability and structure | 15% |
| Input validation and error handling | 10% |
| Testing and documentation | 10% |
| **Total** | **100%** |

---

## 📤 Submission Guidelines

For each exercise:

1. Create a Python file or notebook
2. Include the exercise number and title
3. Add a short explanation of your approach
4. Write the solution and test it
5. Record any assumptions or edge cases
6. Save your work in the repository
7. Commit and push your changes to GitHub

**Folder layout:**

```
01-Python-Basics/
├── README.md          ← you are here
├── practice.md        ← exercise statements
├── examples.ipynb     ← worked examples
├── practice.ipynb     ← your own attempts
├── solutions.ipynb    ← reference solutions
├── quiz.md            ← self-check quiz
└── resources.md       ← further reading
```

Keep exercise statements in `practice.md`, use `practice.ipynb` for your attempts, and store reference solutions separately in `solutions.ipynb`.

---

## ✅ Completion Checklist

- [ ] Use Python variables and data types
- [ ] Perform calculations with operators
- [ ] Implement conditional logic
- [ ] Solve problems using loops
- [ ] Manipulate strings and collections
- [ ] Write reusable functions
- [ ] Handle exceptions and files
- [ ] Solve intermediate programming problems
- [ ] Build at least two mini-projects
- [ ] Test, document, and publish your code on GitHub

---

## ➡️ Next Steps

Continue with the next topics in the roadmap:

1. [Variables and Data Types](../02-Variables-and-Data-Types/)
2. [Data Structures](../06-Data-Structures/)
3. [Functions](../07-Functions/)
4. [File Handling](../09-File-Handling/) and [Exception Handling](../10-Exception-Handling/)
5. [Object-Oriented Programming](../11-OOP/)
6. [NumPy](../../03-NumPy/) and [Pandas](../../04-Pandas-and-Data-Wrangling/)

> **Learning principle:** Understand the concept, solve the problem independently, test your solution, then improve your code. Consistent hands-on practice is the foundation of programming proficiency.

---

[⬅️ Previous: Git and GitHub](../../00-Getting-Started/06-Git-and-GitHub/) · [⬅️ Back to Python Programming](../README.md) · [🏠 Main Roadmap](../../README.md)
