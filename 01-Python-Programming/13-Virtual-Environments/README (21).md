# Python Virtual Environments

A Python virtual environment is an isolated directory that contains a Python interpreter and its own package installation area. It helps projects use different dependency versions without interfering with other projects or the system Python installation.

## Learning objectives

- Explain why virtual environments are useful.
- Create, activate, deactivate, and remove an environment.
- Install and inspect project dependencies with `pip`.
- Save and recreate dependencies using a requirements file.
- Recognize common setup problems across Windows, macOS, and Linux.

## 1. Why use a virtual environment?

Without isolation, installing or upgrading a package for one project can affect another project. A virtual environment keeps project dependencies separate.

Example: Project A may require one version of `requests`, while Project B needs a different version. Each project can maintain its own environment.

## 2. Check Python and pip

```bash
python --version
python -m pip --version
```

On some systems, use `python3` instead of `python`. On Windows, the Python launcher may also be available:

```powershell
py --version
py -m pip --version
```

Using `python -m pip` helps ensure that `pip` belongs to the selected Python interpreter.

## 3. Create a virtual environment

Run these commands from your project directory.

**Windows PowerShell or Command Prompt**

```powershell
py -m venv .venv
```

**macOS / Linux**

```bash
python3 -m venv .venv
```

The name `.venv` is a common convention, but you can choose another name. The environment directory should normally not be committed to Git.

## 4. Activate the environment

**Windows PowerShell**

```powershell
.\.venv\Scripts\Activate.ps1
```

**Windows Command Prompt**

```bat
.venv\Scripts\activate.bat
```

**macOS / Linux (bash or zsh)**

```bash
source .venv/bin/activate
```

After activation, the environment name often appears in the terminal prompt. Verify which Python is active:

```bash
python --version
python -m pip --version
```

To see the interpreter path:

```bash
python -c "import sys; print(sys.executable)"
```

## 5. Install and inspect packages

With the environment active:

```bash
python -m pip install requests
python -m pip list
python -m pip show requests
```

Upgrade pip in the active environment if needed:

```bash
python -m pip install --upgrade pip
```

Avoid using administrator or `sudo` installation commands for ordinary project dependencies when an environment is active.

## 6. Save dependencies

Record installed project packages in `requirements.txt`:

```bash
python -m pip freeze > requirements.txt
```

Install them in another environment:

```bash
python -m pip install -r requirements.txt
```

For a small project, you may maintain a curated `requirements.txt` with only direct dependencies. `pip freeze` records the installed package set, including transitive dependencies, which can be useful for reproducing an environment.

## 7. Deactivate and recreate

Deactivate the current environment:

```bash
deactivate
```

Remove the `.venv` directory when it is no longer needed, then create it again and reinstall dependencies from the requirements file. Do not manually copy a virtual environment between machines; recreate it on the target system.

## 8. Example project workflow

```bash
# Create a project directory and enter it
mkdir demo-project
cd demo-project

# Create and activate environment (macOS/Linux example)
python3 -m venv .venv
source .venv/bin/activate

# Install a package and record dependencies
python -m pip install requests
python -m pip freeze > requirements.txt

# Verify and leave the environment
python -c "import requests; print(requests.__version__)"
deactivate
```

Use the activation command for your own operating system.

## 9. Common issues

### `No module named venv`

On some Linux distributions, the venv component may need to be installed using the system package manager. Install the package appropriate to your Python version and distribution, then retry.

### PowerShell says script execution is disabled

PowerShell execution policy can block activation scripts. Follow your organization's policy and use an approved approach. You can use Command Prompt activation instead, or invoke the environment's Python directly without activating it:

```powershell
.\.venv\Scripts\python.exe -m pip install requests
```

### `pip` installs into the wrong Python

Use the interpreter explicitly:

```bash
python -m pip install package-name
python -c "import sys; print(sys.executable)"
```

### Environment works on one computer but not another

Recreate it using the target computer's Python version and install dependencies from `requirements.txt`. Native packages may require additional operating-system tools.

### Environment folder appears in Git

Add `.venv/` to `.gitignore`. If the folder was already tracked, removing it from Git's index requires an additional `git rm --cached` step; be careful not to delete local files unintentionally.

## 10. Best practices

- Create one environment per project.
- Use `.venv` as a recognizable local directory name.
- Add `.venv/` to `.gitignore`.
- Keep a dependency file such as `requirements.txt`.
- Use `python -m pip` to target the intended interpreter.
- Recreate environments rather than copying them between machines.
- Use supported Python versions and document any version constraints.
- Do not commit secrets, local configuration, or the environment directory.

## Quick reference

| Task | Command |
|---|---|
| Create | `python -m venv .venv` |
| Activate on Windows PowerShell | `.\.venv\Scripts\Activate.ps1` |
| Activate on Windows CMD | `.venv\Scripts\activate.bat` |
| Activate on macOS/Linux | `source .venv/bin/activate` |
| Install a package | `python -m pip install package-name` |
| Save dependencies | `python -m pip freeze > requirements.txt` |
| Install saved dependencies | `python -m pip install -r requirements.txt` |
| Leave environment | `deactivate` |

## Summary

Virtual environments isolate dependencies and make Python projects easier to maintain. Create an environment for each project, install packages inside it, record dependencies, and recreate the environment when moving to another machine.
