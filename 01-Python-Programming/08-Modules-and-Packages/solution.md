# Reference Solutions — 08 Modules and Packages

These are sample answers. File-based exercises may have other valid layouts.

1. A **module** is typically one `.py` file; a **package** groups related modules in a directory.
2. `import math; print(math.sqrt(144))`
3. `from statistics import mean; print(mean([3, 6, 9]))`
4. `import random as rnd; print(rnd.randint(1, 10))`
5. `from pathlib import Path; print(Path.cwd())`
6. `temperature.py`: `def celsius_to_fahrenheit(c): return c * 9 / 5 + 32`
7. In the other file: `import temperature; print(temperature.celsius_to_fahrenheit(20))`
8. When executed directly, `__name__` is normally `"__main__"`.
9. `def main(): print("Demo")` followed by `if __name__ == "__main__": main()`
10. Example:
    ```text
    reports/
    ├── __init__.py
    ├── csv_tools.py
    └── pdf_tools.py
    ```
11. `from reports.csv_tools import export_report`
12. `__init__.py` can initialize a package and makes a traditional package explicit; it may also expose selected package-level names.
13. Absolute: `from reports.csv_tools import export_report`; relative inside a package: `from .csv_tools import export_report`.
14. Wildcard imports make name origins unclear and can overwrite existing names.
15. `from datetime import date; print(date.today())`
16. `import json; json.dumps({"name": "Asha", "active": True})`
17. `import json; json.loads('{"name": "Asha"}')`
18. `from collections import Counter; print(Counter("banana"))`
19. `from statistics import median; print(median([8, 2, 5, 1, 9]))` outputs `5`.
20. `from pathlib import Path; print(Path("README.md").exists())`
21. `python -m venv .venv`
22. `.venv\Scripts\Activate.ps1`
23. `source .venv/bin/activate`
24. `python -m pip install requests`
25. `python -m pip freeze`
26. `python -m pip freeze > requirements.txt`
27. The module may not be installed in the active environment, or Python cannot find it on its import path. Check the interpreter and project layout.
28. A local `random.py` can be imported instead of the standard-library `random` module. Rename it and remove any associated `__pycache__` entry if needed.
29. A circular import occurs when modules depend on each other during initialization, so one may access names before they have been defined.
30. `python -c "import sys; print(sys.executable)"`
31. `python -m pip` runs pip through the selected interpreter, reducing ambiguity when multiple Python installations exist.
32. Example `math_tools.py`: `def add(a, b): return a + b`; then `if __name__ == "__main__": print(add(2, 3))`.
33. Relative imports are appropriate when modules are imported as part of a package, such as `from .cleaning import clean`.
34. Refactor shared code into a third module; redesign module responsibilities; use a local import only when it is a justified workaround.
35. Example:
    ```text
    project/
    ├── .venv/              # local environment; do not commit
    ├── requirements.txt
    └── src/
        └── analytics/
            ├── __init__.py
            ├── cleaning.py
            └── reports.py
    ```
    Activate `.venv`, install dependencies from `requirements.txt`, and run the project from its intended import root.
