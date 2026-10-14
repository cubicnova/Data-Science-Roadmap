# Python Virtual Environments — Reference Solutions

Commands may vary slightly by Python installation and shell. These examples use `.venv` as the environment directory.

```powershell
# 1. Environments isolate dependencies and reduce conflicts between projects.

# 2. Check Python version
python --version
# Windows alternative
py --version

# 3. Check pip for the selected interpreter
python -m pip --version

# 4. Create and enter project directory (shell-dependent)
mkdir weather-app
cd weather-app

# 5. Windows environment creation
py -m venv .venv

# 6. macOS/Linux environment creation
python3 -m venv .venv

# 7. Activate in PowerShell
.\.venv\Scripts\Activate.ps1

# 8. Activate in Command Prompt
.venv\Scripts\activate.bat

# 9. Activate in macOS/Linux bash/zsh
source .venv/bin/activate

# 10. Leave the environment
deactivate

# 11. Print the interpreter path
python -c "import sys; print(sys.executable)"

# 12. Install requests
python -m pip install requests

# 13. List packages
python -m pip list

# 14. Show package details
python -m pip show requests

# 15. Upgrade pip
python -m pip install --upgrade pip

# 16. Save installed package versions
python -m pip freeze > requirements.txt

# 17. Install saved dependencies in a new active environment
python -m pip install -r requirements.txt

# 18. pip freeze captures the installed package set; a curated requirements
# file can focus on direct dependencies and intentional version constraints.

# 19. Example .gitignore contents:
# .venv/
# __pycache__/
# *.py[cod]

# 20. The folder is machine-specific, can be large, and is reproducible from
# Python plus a dependency file. Commit dependency metadata, not the environment.

# 21. Run the environment's Python directly on Windows (PowerShell)
.\.venv\Scripts\python.exe -m pip install requests

# 22. Follow organizational policy; use Command Prompt activation or the
# environment's Python executable directly if PowerShell activation is blocked.

# 23. Install the distribution's venv package for the matching Python version
# using the system package manager, then retry creation.

# 24. Example after creating and activating .venv:
python -m pip install requests
python -c "import requests; print(requests.__version__)"

# 25. Recreate from saved dependencies
python -m venv .venv
# Activate using the appropriate command for the operating system
python -m pip install -r requirements.txt

# 26. Environments can contain absolute paths and platform-specific binaries;
# recreate them on the target system instead.

# 27. From each project directory, create a local environment:
python -m venv .venv
# Repeat separately inside the second project's directory.

# 28. Use `python -c "import sys; print(sys.executable)"` before and after
# activation. The active environment should point to its own interpreter.

# 29. `python -m pip` binds pip to the selected Python interpreter, reducing
# the chance of installing into a different Python installation.

# 30. Example README setup section:
# 1. Create: `py -m venv .venv` on Windows or `python3 -m venv .venv`
#    on macOS/Linux.
# 2. Activate with the matching shell command.
# 3. Install dependencies: `python -m pip install -r requirements.txt`.

# 31. `deactivate` removes the environment's activation from the current shell;
# it does not delete the environment directory.

# 32. Deactivate, delete only `.venv`, recreate it, and reinstall requirements.
# Keep source files and `requirements.txt`.

# 33. Creation may use `py` versus `python3`; activation paths and direct
# interpreter paths differ between Windows and macOS/Linux.

# 34. Check the active interpreter, activate the correct environment, install
# requirements, verify the package is listed, and inspect the import error.

# 35. Example workflow (macOS/Linux):
python3 -m venv .venv
source .venv/bin/activate
python -m pip install requests
python -c "import requests; print(requests.__version__)"
python -m pip freeze > requirements.txt
```

## Notes

- Do not run all shell commands as one Python code cell; these are terminal commands.
- Use the activation command that matches your shell.
- A `requirements.txt` file should be reviewed before sharing, particularly if it includes local or platform-specific dependencies.
