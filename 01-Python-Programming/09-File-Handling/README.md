# 09 – File Handling in Python

## Overview
File handling lets Python programs read, create, update, and organize data stored in files. It is useful for logs, CSV datasets, configuration files, reports, and data-science workflows.

## Learning objectives
By the end of this module, you should be able to:
- Open and close files safely.
- Read and write text files.
- Understand file modes and encodings.
- Use context managers (`with open(...)`).
- Work with paths using `pathlib`.
- Handle common file exceptions.
- Read and write CSV, JSON, and binary files.
- Process larger files line by line.
- Apply safe practices when working with user-provided paths and files.

## 1. Opening a file
```python
file = open("notes.txt", "r", encoding="utf-8")
content = file.read()
file.close()
```
This works, but an exception before `close()` could leave the file open. Prefer a context manager.

## 2. The `with` statement
```python
with open("notes.txt", "r", encoding="utf-8") as file:
    content = file.read()
```
The file is closed automatically when the block exits, including when an exception occurs.

## 3. File modes

| Mode | Meaning |
|---|---|
| `r` | Read; file must exist |
| `w` | Write; creates or truncates a file |
| `a` | Append; creates file if missing |
| `x` | Create exclusively; fails if file exists |
| `b` | Binary mode modifier, e.g. `rb` |
| `t` | Text mode modifier; default |
| `+` | Read and write modifier, e.g. `r+` |

Be careful: `w` truncates an existing file. Use `a` when you intend to append.

## 4. Reading text
```python
with open("notes.txt", "r", encoding="utf-8") as file:
    all_text = file.read()

with open("notes.txt", "r", encoding="utf-8") as file:
    first_line = file.readline()

with open("notes.txt", "r", encoding="utf-8") as file:
    lines = file.readlines()
```
For larger files, iterate over the file object instead of loading everything into memory:
```python
with open("large.log", encoding="utf-8") as file:
    for line in file:
        process_line = line.rstrip("\n")
        # process_line can now be inspected or parsed
```

## 5. Writing and appending
```python
with open("report.txt", "w", encoding="utf-8") as file:
    file.write("Report generated\n")

with open("report.txt", "a", encoding="utf-8") as file:
    file.write("Another entry\n")
```
`write()` does not automatically add a newline. Include `\n` when required.

## 6. File paths with `pathlib`
```python
from pathlib import Path

path = Path("data") / "sample.txt"
print(path.name)
print(path.suffix)
print(path.exists())
```
Use `Path` to compose paths portably across operating systems. Relative paths are interpreted from the process's current working directory, not necessarily from the script's directory.

## 7. Creating directories and listing files
```python
from pathlib import Path

folder = Path("output")
folder.mkdir(parents=True, exist_ok=True)

for path in folder.iterdir():
    print(path.name)
```
Use `glob()` or `rglob()` to find files by patterns. Avoid recursively traversing untrusted or very large directory trees without limits.

## 8. Exceptions and robust file operations
Common exceptions include:
- `FileNotFoundError`: the requested file does not exist.
- `PermissionError`: the process lacks permission.
- `IsADirectoryError`: a directory was used where a file was expected.
- `UnicodeDecodeError`: bytes cannot be decoded with the chosen encoding.

```python
from pathlib import Path

path = Path("notes.txt")
try:
    text = path.read_text(encoding="utf-8")
except FileNotFoundError:
    print("File was not found.")
except UnicodeDecodeError:
    print("The file is not valid UTF-8 text.")
```
Catch specific exceptions you can meaningfully handle. Do not silently ignore all exceptions.

## 9. CSV files
Use Python's `csv` module instead of splitting rows manually, because quoted fields may contain commas.

```python
import csv

with open("people.csv", newline="", encoding="utf-8") as file:
    reader = csv.DictReader(file)
    rows = list(reader)
```

Writing:
```python
import csv

rows = [{"name": "Asha", "score": 91}, {"name": "Ravi", "score": 84}]
with open("scores.csv", "w", newline="", encoding="utf-8") as file:
    writer = csv.DictWriter(file, fieldnames=["name", "score"])
    writer.writeheader()
    writer.writerows(rows)
```

## 10. JSON files
JSON is commonly used for structured data and configuration.

```python
import json

data = {"project": "roadmap", "completed": True}
with open("config.json", "w", encoding="utf-8") as file:
    json.dump(data, file, indent=2)

with open("config.json", encoding="utf-8") as file:
    loaded = json.load(file)
```
`json.dump`/`json.load` work with file objects; `json.dumps`/`json.loads` work with strings.

## 11. Binary files
Use binary mode for images, PDFs, archives, and other non-text data.

```python
with open("source.bin", "rb") as source:
    data = source.read()

with open("copy.bin", "wb") as destination:
    destination.write(data)
```
For large files, copy in chunks instead of reading the entire file into memory.

## 12. File metadata and safe practices
```python
from pathlib import Path

path = Path("notes.txt")
if path.exists() and path.is_file():
    print(path.stat().st_size)
```
Best practices:
- Use context managers.
- Specify an encoding such as UTF-8 for text.
- Validate file names and paths that come from users.
- Avoid overwriting important files unintentionally.
- Do not deserialize untrusted pickle files; loading pickle can execute arbitrary code.
- Use temporary files and atomic replacement patterns when robust updates are needed.
- Keep secrets out of logs and exported files.

## Common mistakes
- Forgetting to close a file.
- Using `w` when `a` was intended.
- Omitting `encoding` for text.
- Assuming the current working directory is the script's directory.
- Splitting CSV rows on commas instead of using `csv`.
- Reading a huge file entirely into memory.
- Catching every exception and hiding the real error.
- Treating binary data as UTF-8 text.

## Quick reference

| Task | Example |
|---|---|
| Read all text | `Path("notes.txt").read_text(encoding="utf-8")` |
| Write text | `Path("notes.txt").write_text("Hello", encoding="utf-8")` |
| Append | `open("log.txt", "a", encoding="utf-8")` |
| Create directory | `Path("out").mkdir(parents=True, exist_ok=True)` |
| Read CSV | `csv.DictReader(file)` |
| Read JSON | `json.load(file)` |
| File size | `Path("file.txt").stat().st_size` |

## Module files
- `README.md` — theory, syntax, and examples
- `practice.md` — 35 exercises
- `examples.ipynb` — runnable notebook examples
- `solutions.md` — reference answers
