# Python Virtual Environments — Practice Exercises

Use a terminal on your operating system for command-line tasks. If you cannot run a task, write the correct commands and explain what each one does.

1. Explain two reasons for using a virtual environment in Python.
2. Check the installed Python version from the terminal.
3. Check which pip version belongs to the current Python interpreter.
4. Create a project folder named `weather-app` and enter it.
5. Create a virtual environment named `.venv` on Windows.
6. Create a virtual environment named `.venv` on macOS or Linux.
7. Write the PowerShell command used to activate `.venv`.
8. Write the Command Prompt command used to activate `.venv`.
9. Write the macOS/Linux command used to activate `.venv`.
10. Deactivate an active virtual environment.
11. Print the executable path of the Python interpreter currently running.
12. Install the `requests` package into the active environment.
13. List all packages installed in the active environment.
14. Display package information for `requests`.
15. Upgrade pip in the active environment.
16. Save installed packages to a file named `requirements.txt`.
17. Install packages listed in `requirements.txt` into a new environment.
18. Explain the difference between `pip freeze` and manually maintaining direct dependencies.
19. Create a `.gitignore` file that excludes `.venv/` and Python bytecode caches.
20. Explain why a `.venv` directory should normally not be committed to Git.
21. Show how to run the environment's Python directly on Windows without activation.
22. Explain one safe way to handle a PowerShell activation-policy error.
23. Describe what to do if Python reports `No module named venv` on Linux.
24. Create an environment, activate it, install `requests`, and verify the import.
25. Write a sequence of commands to recreate an environment from `requirements.txt`.
26. Explain why copying a virtual environment to another computer is discouraged.
27. Create two separate environment folders for two sample projects.
28. Check the Python executable path before and after activation and compare the results.
29. Explain why `python -m pip install` is often safer than running `pip install` alone.
30. Write a short setup section for a README explaining how a teammate can create and activate `.venv`.
31. Explain what happens when you run `deactivate`.
32. Describe how to remove a virtual environment and recreate it without deleting project source files.
33. Identify which commands in a project setup workflow are operating-system-specific.
34. Write a simple troubleshooting checklist for a package import failure.
35. Create a clean project workflow that creates `.venv`, installs dependencies, verifies an import, and records requirements.

## Challenge

Write a `README.md` setup section for a small Python project that works for both Windows and macOS/Linux users.
