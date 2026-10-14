# 08 – Modules and Packages in Python

## Overview
A **module** is a Python file (`.py`) containing reusable code such as functions, classes, and variables. A **package** organizes related modules into a directory. Modules and packages help make projects easier to maintain, reuse, test, and distribute.

## Learning objectives
By the end of this module, you should be able to:
- Explain the difference between a module, package, and library.
- Import modules using common import styles.
- Create and import your own modules.
- Use aliases and selected imports safely.
- Understand `__name__ == "__main__"`.
- Organize code into packages and subpackages.
- Explore Python's standard library.
- Install third-party packages with `pip` and use virtual environments.
- Diagnose common import errors.

## 1. Module, package, and library
- **Module:** one Python source file, such as `math_tools.py`.
- **Package:** a directory that groups related modules; traditional packages commonly include `__init__.py`.
- **Library:** a broader collection of reusable code, often containing multiple modules or packages.

## 2. Importing standard-library modules
```python
import math
print(math.sqrt(25))

import random as rnd
print(rnd.randint(1, 6))

from pathlib import Path
print(Path(".").resolve())
```
Prefer `import module` or `import module as alias` for clarity. Avoid wildcard imports (`from module import *`) because they make names harder to trace.

## 3. Importing selected names
```python
from statistics import mean, median
print(mean([2, 4, 6]))
print(median([1, 9, 3]))
```
A selected import lets you use the imported names directly. Be careful not to overwrite built-in or imported names accidentally.

## 4. Creating your own module
Suppose a folder contains these files:

```text
project/
├── main.py
└── math_tools.py
```

`math_tools.py`:
```python
def add(a, b):
    return a + b

def multiply(a, b):
    return a * b
```

`main.py`:
```python
import math_tools

print(math_tools.add(3, 4))
```
When both files are in the same importable location, Python can import `math_tools`.

## 5. The `__name__` variable and main guard
When a Python file is run directly, its `__name__` is usually `"__main__"`. When imported, its `__name__` is the module name.

```python
def main():
    print("Run the program here")

if __name__ == "__main__":
    main()
```
The main guard prevents script-only behavior from running automatically when the module is imported.

## 6. Packages and `__init__.py`
Example structure:

```text
analytics/
├── __init__.py
├── cleaning.py
└── statistics_tools.py
```

Example import:
```python
from analytics.cleaning import trim_text
```
`__init__.py` can be empty or contain package initialization code. Modern Python also supports namespace packages without it, but including it is often clearer for beginner projects.

## 7. Absolute and relative imports
An absolute import starts from the package's importable root:
```python
from analytics.cleaning import trim_text
```
A relative import uses dots within a package:
```python
from .cleaning import trim_text
```
Relative imports generally work when code is run as part of a package, not when a package file is executed directly as a standalone script.

## 8. Standard library essentials
Useful modules include:
- `math` — mathematical functions.
- `statistics` — mean, median, and other descriptive statistics.
- `random` — pseudo-random choices and numbers; not for security tokens.
- `datetime` — date and time operations.
- `pathlib` — filesystem paths.
- `json` — JSON encoding and decoding.
- `collections` — specialized containers.
- `os` — operating-system interfaces.
- `re` — regular expressions.

## 9. Third-party packages and `pip`
Install packages into the intended Python environment:
```bash
python -m pip install requests
python -m pip show requests
python -m pip freeze
```
Use `python -m pip` so pip is associated with the selected Python interpreter. Follow your organization's policy before installing software on managed devices.

## 10. Virtual environments
A virtual environment isolates a project's installed packages.

Create one:
```bash
python -m venv .venv
```

Activate it:
- Windows PowerShell: `.venv\Scripts\Activate.ps1`
- Windows Command Prompt: `.venv\Scripts\activate.bat`
- macOS/Linux: `source .venv/bin/activate`

Deactivate with:
```bash
deactivate
```
Do not commit the `.venv/` directory. A dependency file can record packages, for example `python -m pip freeze > requirements.txt`.

## 11. Inspecting imports and troubleshooting
```python
import math
print(math.__name__)
print(math.__file__)  # often shows the module file; built-ins may not have a normal path
```
Common errors:
- `ModuleNotFoundError`: module is not installed or is not on the import path.
- `ImportError`: a requested name cannot be imported or imports conflict.
- Circular import: modules depend on each other during initialization.
- Name shadowing: a local file named `random.py`, `json.py`, or `statistics.py` hides the standard-library module.

Helpful checks:
```bash
python --version
python -m pip --version
python -c "import sys; print(sys.executable)"
```

## Best practices
- Give modules descriptive names and avoid names that shadow standard-library modules.
- Keep each module focused on a related responsibility.
- Use explicit imports and avoid wildcard imports.
- Use a virtual environment per project.
- Document external dependencies in `requirements.txt` or project metadata.
- Keep import-time code lightweight; put runnable behavior behind a main guard.
- Avoid circular dependencies by reconsidering module boundaries.

## Quick reference

| Task | Example |
|---|---|
| Import a module | `import math` |
| Import with alias | `import numpy as np` |
| Import selected names | `from statistics import mean` |
| Own module | `import math_tools` |
| Main guard | `if __name__ == "__main__":` |
| Create venv | `python -m venv .venv` |
| Install package | `python -m pip install requests` |
| Export dependencies | `python -m pip freeze > requirements.txt` |

## Module files
- `README.md` — theory, syntax, and examples
- `practice.md` — 35 exercises
- `examples.ipynb` — runnable notebook examples
- `solutions.md` — reference answers
