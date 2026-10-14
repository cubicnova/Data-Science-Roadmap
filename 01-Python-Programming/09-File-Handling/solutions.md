# Reference Solutions — 09 File Handling

Sample answers follow. For file operations, run examples in a test directory.

```python
from pathlib import Path
import csv
import json
from datetime import datetime
import shutil

# 1
with open("hello.txt", "w", encoding="utf-8") as file:
    file.write("Hello, Python!")

# 2
with open("hello.txt", encoding="utf-8") as file:
    print(file.read())

# 3
with open("notes.txt", encoding="utf-8") as file:
    first_line = file.readline()

# 4
with open("large.log", encoding="utf-8") as file:
    for line in file:
        print(line.rstrip("\n"))

# 5
with open("log.txt", "a", encoding="utf-8") as file:
    file.write("New log entry\n")

# 6
# "w" creates or truncates; "a" writes at the end.

# 7
with open("new_file.txt", "x", encoding="utf-8") as file:
    file.write("Created once")

# 8
with open("three_lines.txt", "w", encoding="utf-8") as file:
    file.writelines(["Line 1\n", "Line 2\n", "Line 3\n"])

# 9
print(Path("data.csv").suffix)

# 10
print(Path("report.txt").exists())

# 11
Path("output/reports").mkdir(parents=True, exist_ok=True)

# 12
for path in Path(".").glob("*.txt"):
    print(path.name)

# 13
try:
    content = Path("missing.txt").read_text(encoding="utf-8")
except FileNotFoundError:
    content = ""

# 14
# UTF-8 explicitly defines how text is encoded and decoded across systems.

# 15
with open("notes.txt", encoding="utf-8") as file:
    line_count = sum(1 for _ in file)

# 16
text = Path("notes.txt").read_text(encoding="utf-8")
word_count = len(text.split())

# 17
source = Path("input.txt")
destination = Path("output.txt")
destination.write_text(
    source.read_text(encoding="utf-8").replace("old", "new"),
    encoding="utf-8",
)

# 18
with open("source.bin", "rb") as src, open("copy.bin", "wb") as dst:
    shutil.copyfileobj(src, dst)

# 19
with open("people.csv", newline="", encoding="utf-8") as file:
    rows = list(csv.DictReader(file))

# 20
rows = [{"name": "Asha", "score": 91}, {"name": "Ravi", "score": 84}]
with open("scores.csv", "w", newline="", encoding="utf-8") as file:
    writer = csv.DictWriter(file, fieldnames=["name", "score"])
    writer.writeheader()
    writer.writerows(rows)

# 21
data = {"project": "roadmap", "completed": True}
with open("config.json", "w", encoding="utf-8") as file:
    json.dump(data, file, indent=2)

# 22
with open("config.json", encoding="utf-8") as file:
    loaded = json.load(file)

# 23
# json.dump(obj, file) writes to a file-like object.
# json.dumps(obj) returns a JSON string.

# 24
size_bytes = Path("notes.txt").stat().st_size

# 25
path = Path("some_path")
print(path.is_file(), path.is_dir())

# 26
with open("app.log", "a", encoding="utf-8") as file:
    file.write(f"{datetime.now().isoformat()} Event recorded\n")

# 27
with open("large_source.bin", "rb") as src, open("large_copy.bin", "wb") as dst:
    while chunk := src.read(1024 * 1024):
        dst.write(chunk)

# 28
# The bytes cannot be decoded using the selected encoding.

# 29
# Pickle is unsafe for untrusted input because loading may execute code.

# 30
try:
    with open("data.json", encoding="utf-8") as file:
        data = json.load(file)
except FileNotFoundError:
    data = {}

# 31
input_path = Path("data") / "input.csv"

# 32
with open("notes.txt", encoding="utf-8") as file:
    lines = [line.rstrip("\n") for line in file]

# 33
for path in Path("data").rglob("*.csv"):
    print(path)

# 34
# CSV fields can contain commas inside quotes; use the csv module.

# 35
def read_text_file(path):
    return Path(path).read_text(encoding="utf-8")
```

## Notes
- Use temporary directories for exercises that create, overwrite, or delete files.
- Always specify an encoding for text files.
- `Path.read_text()` and `Path.write_text()` are concise alternatives for straightforward text operations.
- Use `csv` and `json` libraries instead of parsing their formats manually.
