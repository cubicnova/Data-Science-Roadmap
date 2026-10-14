# Practice Exercises — 08 Modules and Packages

Complete all 35 exercises. For file-creation tasks, write the code in separate `.py` files and test the imports from the project root.

## 1. Explain the difference between a module and a package.

**Hint:** A module is usually one `.py` file; a package groups modules.

## 2. Import `math` and calculate the square root of 144.

**Hint:** Use `math.sqrt(144)`.

## 3. Import `statistics.mean` and calculate the mean of 3, 6, and 9.

**Hint:** Use a selected import.

## 4. Import `random` using the alias `rnd` and generate an integer from 1 to 10.

**Hint:** Call `rnd.randint(1, 10)`.

## 5. Import `Path` from `pathlib` and print the current directory path.

**Hint:** Use `Path.cwd()`.

## 6. Write a module file named `temperature.py` with a Celsius-to-Fahrenheit function.

**Hint:** Define a reusable function in that file.

## 7. Show how another file imports and calls a function from `temperature.py`.

**Hint:** Use `import temperature` or a selected import.

## 8. Explain what `__name__` contains when a file runs directly.

**Hint:** It is usually `"__main__"`.

## 9. Write a main guard that calls a `main()` function.

**Hint:** Use `if __name__ == "__main__":`.

## 10. Design a package structure with `reports` containing `csv_tools.py` and `pdf_tools.py`.

**Hint:** Draw the directory tree and include `__init__.py`.

## 11. Write an import for `export_report` from `reports/csv_tools.py`.

**Hint:** Use `from reports.csv_tools import export_report`.

## 12. Explain the purpose of `__init__.py`.

**Hint:** It marks/initializes a traditional package.

## 13. Compare absolute and relative imports with examples.

**Hint:** Use a package-root path versus a leading dot.

## 14. Explain why `from module import *` is usually discouraged.

**Hint:** It obscures where names originate and can cause collisions.

## 15. Use `datetime` to print the current date.

**Hint:** Use `date.today()`.

## 16. Use `json` to serialize a dictionary to a JSON string.

**Hint:** Call `json.dumps`.

## 17. Use `json` to parse a JSON string into Python data.

**Hint:** Call `json.loads`.

## 18. Use `collections.Counter` to count characters in a word.

**Hint:** Pass the word to `Counter`.

## 19. Use `statistics.median` on `[8, 2, 5, 1, 9]`.

**Hint:** Import or qualify `median`.

## 20. Use `pathlib` to check whether a file named `README.md` exists in the current directory.

**Hint:** Use `Path('README.md').exists()`.

## 21. Write the terminal command to create a virtual environment named `.venv`.

**Hint:** Use `python -m venv .venv`.

## 22. Give the PowerShell command to activate `.venv` on Windows.

**Hint:** Use `.venv\Scripts\Activate.ps1`.

## 23. Give the macOS/Linux command to activate `.venv`.

**Hint:** Use `source .venv/bin/activate`.

## 24. Write a command to install `requests` for the current Python interpreter.

**Hint:** Use `python -m pip install requests`.

## 25. Write a command to list installed packages and versions.

**Hint:** Use `python -m pip freeze`.

## 26. Write a command to save installed package versions to `requirements.txt`.

**Hint:** Redirect `pip freeze` output.

## 27. Explain `ModuleNotFoundError` and give two possible causes.

**Hint:** Check installation, environment, and import path.

## 28. Explain how a local `random.py` file can cause an import problem.

**Hint:** It can shadow the standard-library module.

## 29. Describe a circular import.

**Hint:** Two modules import each other while initializing.

## 30. Show a command that prints the executable path for the active Python interpreter.

**Hint:** Use `python -c "import sys; print(sys.executable)"`.

## 31. Explain why `python -m pip` is often safer than `pip` alone.

**Hint:** It ties pip to the selected interpreter.

## 32. Write a module with an `add(a, b)` function and a main guard for a demo.

**Hint:** Keep definitions reusable and demo under the guard.

## 33. Explain when a relative import such as `from .cleaning import clean` is appropriate.

**Hint:** When the module runs as part of a package.

## 34. List three ways to reduce circular-import problems.

**Hint:** Refactor boundaries, move shared code, or import locally when appropriate.

## 35. Describe a clean project layout using a package, `requirements.txt`, and a virtual environment.

**Hint:** Keep source files and dependency metadata separate from `.venv`.

