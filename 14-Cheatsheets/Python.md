# Python Cheatsheet

A practical Python reference for beginners, developers, data analysts, data scientists, and machine learning engineers.

Python is a high-level, general-purpose programming language widely used for automation, data analysis, machine learning, artificial intelligence, web development, and scripting.

## Table of Contents

- [1. Installation and Setup](#1-installation-and-setup)
- [2. Python Syntax and Comments](#2-python-syntax-and-comments)
- [3. Variables and Data Types](#3-variables-and-data-types)
- [4. Type Conversion](#4-type-conversion)
- [5. Operators](#5-operators)
- [6. Strings](#6-strings)
- [7. Lists](#7-lists)
- [8. Tuples](#8-tuples)
- [9. Sets](#9-sets)
- [10. Dictionaries](#10-dictionaries)
- [11. Conditional Statements](#11-conditional-statements)
- [12. Loops](#12-loops)
- [13. List, Dictionary, and Set Comprehensions](#13-list-dictionary-and-set-comprehensions)
- [14. Functions](#14-functions)
- [15. Lambda, Map, Filter, and Reduce](#15-lambda-map-filter-and-reduce)
- [16. Useful Built-in Functions](#16-useful-built-in-functions)
- [17. Exception Handling](#17-exception-handling)
- [18. File Handling](#18-file-handling)
- [19. Modules and Packages](#19-modules-and-packages)
- [20. Object-Oriented Programming](#20-object-oriented-programming)
- [21. Iterators and Generators](#21-iterators-and-generators)
- [22. Decorators and Context Managers](#22-decorators-and-context-managers)
- [23. Dates and Times](#23-dates-and-times)
- [24. Regular Expressions](#24-regular-expressions)
- [25. Virtual Environments and pip](#25-virtual-environments-and-pip)
- [26. Python for Data Science](#26-python-for-data-science)
- [27. Common Mistakes](#27-common-mistakes)
- [28. Quick Reference](#28-quick-reference)
- [29. Practice Exercises](#29-practice-exercises)
- [30. Official Resources](#30-official-resources)

## 1. Installation and Setup

### Check Python installation

```bash
python --version
```

On some Linux and macOS systems:

```bash
python3 --version
```

### Run a Python script

```bash
python script.py
```

### Install packages

```bash
python -m pip install numpy pandas matplotlib
```

### Start the interactive interpreter

```bash
python
```

Example:

```python
print("Hello, Data Science!")
```

## 2. Python Syntax and Comments

### Variables and statements

```python
name = "CubicNova"
year = 2026

print(name)
print(year)
```

Python uses indentation to define code blocks.

```python
if year >= 2025:
    print("Recent year")
```

### Comments

```python
# Single-line comment

"""
This is a multiline string.
It can serve as a docstring when placed
at the beginning of a module, class, or function.
"""
```

### Naming conventions

```python
student_name = "Amit"     # snake_case
MAX_RETRIES = 3           # Constant convention
```

Use descriptive names. Python identifiers are case-sensitive.

## 3. Variables and Data Types

### Basic data types

```python
age = 25                  # int
price = 99.99             # float
name = "Python"           # str
is_active = True          # bool
result = None             # NoneType
complex_num = 2 + 3j      # complex
```

### Check data types

```python
print(type(age))
print(type(price))
print(isinstance(name, str))
```

### Multiple assignment

```python
a, b, c = 10, 20, 30

x = y = 100
```

### Swap variables

```python
a = 10
b = 20

a, b = b, a
```

### Mutable and immutable objects

| Mutable | Immutable |
|---|---|
| `list` | `int` |
| `dict` | `float` |
| `set` | `str` |
| Many custom objects | `tuple` (when its elements do not mutate) |
| | `bool` |

Mutability describes whether an object's contents can change after creation. A tuple cannot have its element references reassigned, although it can contain mutable objects.

## 4. Type Conversion

```python
int("100")              # 100
float("3.14")           # 3.14
str(123)                # "123"
bool(1)                 # True
list((1, 2, 3))         # [1, 2, 3]
tuple([1, 2, 3])        # (1, 2, 3)
set([1, 1, 2, 3])       # {1, 2, 3}
```

### User input

```python
age = int(input("Enter your age: "))
print(age)
```

`input()` returns a string, so convert the value when numerical operations are required.

## 5. Operators

### Arithmetic operators

```python
a, b = 10, 3

a + b       # 13
a - b       # 7
a * b       # 30
a / b       # 3.333...
a // b      # 3
a % b       # 1
a ** b      # 1000
```

### Comparison operators

```python
a == b
a != b
a > b
a < b
a >= b
a <= b
```

### Logical operators

```python
age = 25

age >= 18 and age <= 60
age < 18 or age > 60
not age < 18
```

### Membership operators

```python
"py" in "python"         # True
10 in [10, 20, 30]      # True
```

### Identity operators

```python
a = None

a is None
a is not None
```

Use `is` to check object identity, especially for singleton values such as `None`. Use `==` to compare values.

### Assignment operators

```python
x = 10
x += 5
x -= 2
x *= 3
x /= 2
```

## 6. Strings

### Create strings

```python
name = "Python"
message = 'Learn Data Science'
multiline = """Multiple
lines of text"""
```

### Indexing and slicing

```python
text = "Python"

text[0]       # 'P'
text[-1]      # 'n'
text[0:3]     # 'Pyt'
text[:4]      # 'Pyth'
text[::2]     # 'Pto'
text[::-1]    # 'nohtyP'
```

### Common string methods

```python
text = "  Python Programming  "

text.lower()
text.upper()
text.title()
text.strip()
text.replace("Python", "Data")
text.split()
text.startswith("  Py")
text.find("Program")
text.count("m")
```

### Join strings

```python
words = ["Python", "for", "Data Science"]

sentence = " ".join(words)
print(sentence)
```

### String formatting

```python
name = "Amit"
score = 92.567

print(f"{name} scored {score:.2f}")
# Amit scored 92.57
```

Formatting examples:

```python
f"{1000:,}"        # 1,000
f"{0.875:.1%}"     # 87.5%
f"{3.14159:.2f}"   # 3.14
```

Strings are immutable. Methods such as `replace()` return a new string.

## 7. Lists

Lists are ordered, mutable collections.

### Create and access lists

```python
numbers = [10, 20, 30, 40]

numbers[0]       # 10
numbers[-1]      # 40
numbers[1:3]     # [20, 30]
```

### Modify lists

```python
numbers.append(50)
numbers.extend([60, 70])
numbers.insert(0, 5)

numbers.remove(20)
last = numbers.pop()
del numbers[0]
```

### Sorting and reversing

```python
numbers = [30, 10, 40, 20]

numbers.sort()
numbers.sort(reverse=True)

sorted_numbers = sorted(numbers)
reversed_numbers = list(reversed(numbers))
```

`list.sort()` modifies the list in place. `sorted()` returns a new sorted list.

### List operations

```python
a = [1, 2, 3]
b = [4, 5, 6]

a + b
a * 2
len(a)
min(a)
max(a)
sum(a)
```

### Copy a list

```python
original = [1, 2, 3]

shallow_copy = original.copy()
another_copy = original[:]
```

Both examples create shallow copies. Nested mutable elements remain shared.

## 8. Tuples

Tuples are ordered collections that cannot be structurally modified after creation.

```python
coordinates = (10, 20)
single_item = (10,)
```

### Access and unpack

```python
point = (5, 10)

x, y = point

print(x)
print(y)
```

### Tuple methods

```python
values = (1, 2, 2, 3)

values.count(2)
values.index(3)
```

### Use cases

- Represent fixed records or coordinates.
- Return multiple values from functions.
- Unpack structured data.
- Use as dictionary keys when all elements are hashable.

## 9. Sets

Sets store unique elements and support mathematical set operations.

```python
numbers = {1, 2, 3, 3}
print(numbers)          # {1, 2, 3}
```

Create an empty set:

```python
empty_set = set()
```

`{}` creates an empty dictionary, not an empty set.

### Modify sets

```python
numbers.add(4)
numbers.update([5, 6])
numbers.remove(2)
numbers.discard(100)
```

`remove()` raises `KeyError` when an element is absent; `discard()` does not.

### Set operations

```python
a = {1, 2, 3}
b = {3, 4, 5}

a | b       # Union
a & b       # Intersection
a - b       # Difference
a ^ b       # Symmetric difference
```

### Remove duplicates

```python
values = [1, 2, 2, 3, 3, 4]

unique_values = set(values)
```

Sets are unordered collections. Do not rely on their iteration order for sorting or positional access.

## 10. Dictionaries

Dictionaries store key-value pairs.

### Create and access

```python
student = {
    "name": "Amit",
    "age": 22,
    "score": 90
}

student["name"]
student.get("age")
student.get("city", "Unknown")
```

### Add, update, and remove

```python
student["city"] = "Indore"
student["score"] = 95

student.update({"age": 23})
student.pop("age")
```

### Iterate over dictionaries

```python
for key in student:
    print(key)

for value in student.values():
    print(value)

for key, value in student.items():
    print(key, value)
```

### Dictionary methods

```python
student.keys()
student.values()
student.items()
student.get("name")
student.setdefault("country", "India")
```

### Dictionary comprehension

```python
squares = {x: x**2 for x in range(1, 6)}
```

Dictionary keys must be hashable. Insertion order is guaranteed in modern Python versions.

## 11. Conditional Statements

### if, elif, else

```python
score = 82

if score >= 90:
    grade = "A"
elif score >= 75:
    grade = "B"
elif score >= 50:
    grade = "C"
else:
    grade = "F"

print(grade)
```

### Ternary expression

```python
age = 20

status = "Adult" if age >= 18 else "Minor"
```

### Match statement

Python 3.10 and later support structural pattern matching.

```python
command = "start"

match command:
    case "start":
        print("Starting")
    case "stop":
        print("Stopping")
    case _:
        print("Unknown command")
```

Use `match` when pattern-based branching improves readability.

## 12. Loops

### For loop

```python
for number in range(5):
    print(number)
```

`range(5)` produces integers from 0 through 4.

```python
for number in range(2, 11, 2):
    print(number)
```

### While loop

```python
count = 1

while count <= 5:
    print(count)
    count += 1
```

### Break and continue

```python
for number in range(10):
    if number == 3:
        continue

    if number == 7:
        break

    print(number)
```

- `break` terminates the nearest loop.
- `continue` skips the rest of the current iteration.
- `pass` is a placeholder statement that does nothing.

### Enumerate

```python
names = ["Amit", "Priya", "Rahul"]

for index, name in enumerate(names, start=1):
    print(index, name)
```

### Zip

```python
names = ["Amit", "Priya"]
scores = [90, 85]

for name, score in zip(names, scores):
    print(name, score)
```

By default, `zip()` stops when the shortest iterable is exhausted.

## 13. List, Dictionary, and Set Comprehensions

### List comprehension

```python
squares = [x**2 for x in range(1, 6)]
```

### Filter values

```python
even_numbers = [x for x in range(10) if x % 2 == 0]
```

### Conditional expression

```python
labels = ["Even" if x % 2 == 0 else "Odd" for x in range(5)]
```

### Dictionary comprehension

```python
squares = {x: x**2 for x in range(1, 6)}
```

### Set comprehension

```python
remainders = {x % 3 for x in range(10)}
```

### Nested comprehension

```python
matrix = [[1, 2], [3, 4]]

flattened = [value for row in matrix for value in row]
```

Use comprehensions for straightforward transformations. Prefer ordinary loops when nested logic becomes difficult to understand.

## 14. Functions

### Define a function

```python
def greet(name):
    return f"Hello, {name}!"

print(greet("Amit"))
```

### Default arguments

```python
def greet(name, message="Welcome"):
    return f"{message}, {name}"

greet("Amit")
greet("Amit", "Hello")
```

### Keyword arguments

```python
def introduce(name, age):
    return f"{name} is {age} years old"

introduce(age=25, name="Priya")
```

### Variable-length arguments

```python
def add_all(*numbers):
    return sum(numbers)

add_all(1, 2, 3, 4)
```

Keyword arguments can be collected with `**kwargs`:

```python
def show_details(**details):
    for key, value in details.items():
        print(key, value)

show_details(name="Amit", role="Data Scientist")
```

### Type hints

```python
def calculate_average(values: list[float]) -> float:
    return sum(values) / len(values)
```

Type hints document expected types but do not automatically enforce them at runtime.

### Docstrings

```python
def square(number: float) -> float:
    """Return the square of a number."""
    return number**2
```

### Avoid mutable default arguments

Avoid:

```python
def add_item(item, items=[]):
    items.append(item)
    return items
```

Prefer:

```python
def add_item(item, items=None):
    if items is None:
        items = []

    items.append(item)
    return items
```

Default argument objects are created once when the function is defined, not on every call.

## 15. Lambda, Map, Filter, and Reduce

### Lambda functions

```python
square = lambda x: x**2

print(square(5))
```

For complex logic, use a regular `def` function.

### Map

```python
numbers = [1, 2, 3, 4]

squares = list(map(lambda x: x**2, numbers))
```

Comprehension alternative:

```python
squares = [x**2 for x in numbers]
```

### Filter

```python
numbers = [1, 2, 3, 4, 5, 6]

evens = list(filter(lambda x: x % 2 == 0, numbers))
```

### Reduce

```python
from functools import reduce

numbers = [1, 2, 3, 4]

product = reduce(lambda x, y: x * y, numbers)
```

`map()` and `filter()` return iterators in Python 3. Convert them to lists when you need a materialized list.

## 16. Useful Built-in Functions

| Function | Purpose |
|---|---|
| `print()` | Display output |
| `input()` | Read user input |
| `len()` | Get length |
| `type()` | Inspect type |
| `isinstance()` | Check type membership |
| `range()` | Generate an integer sequence |
| `enumerate()` | Iterate with indices |
| `zip()` | Combine iterables |
| `sorted()` | Return sorted items |
| `reversed()` | Iterate in reverse |
| `sum()` | Add numeric values |
| `min()` | Find minimum |
| `max()` | Find maximum |
| `abs()` | Absolute value |
| `round()` | Round a number |
| `any()` | Check whether any element is truthy |
| `all()` | Check whether all elements are truthy |
| `map()` | Transform iterable elements |
| `filter()` | Filter iterable elements |
| `isinstance()` | Check whether an object matches a type |

Example:

```python
numbers = [10, 20, 30, 40]

print(sum(numbers))
print(min(numbers))
print(max(numbers))
print(len(numbers))
print(round(3.14159, 2))
```

### Sorting with a key

```python
students = [
    {"name": "Amit", "score": 85},
    {"name": "Priya", "score": 95},
    {"name": "Rahul", "score": 78}
]

ranked = sorted(students, key=lambda student: student["score"], reverse=True)
```

## 17. Exception Handling

### try and except

```python
try:
    number = int(input("Enter a number: "))
    result = 100 / number

except ValueError:
    print("Please enter a valid integer.")

except ZeroDivisionError:
    print("Cannot divide by zero.")
```

### else and finally

```python
try:
    number = int("100")

except ValueError:
    print("Conversion failed.")

else:
    print("Conversion successful:", number)

finally:
    print("Operation completed.")
```

- `except` handles matching exceptions.
- `else` runs when no exception occurs in the `try` block.
- `finally` runs during normal or exceptional control flow.

### Raise an exception

```python
def calculate_square_root_input(number):
    if number < 0:
        raise ValueError("Number cannot be negative")

    return number**0.5
```

Catch specific exceptions whenever possible. Avoid silently swallowing unexpected errors.

## 18. File Handling

### Read a text file

```python
with open("data.txt", "r", encoding="utf-8") as file:
    content = file.read()

print(content)
```

### Read lines

```python
with open("data.txt", "r", encoding="utf-8") as file:
    for line in file:
        print(line.strip())
```

### Write a file

```python
with open("output.txt", "w", encoding="utf-8") as file:
    file.write("Python for Data Science\n")
```

### Append content

```python
with open("output.txt", "a", encoding="utf-8") as file:
    file.write("New entry\n")
```

### File modes

| Mode | Meaning |
|---|---|
| `r` | Read |
| `w` | Write, replacing existing contents |
| `a` | Append |
| `x` | Create a new file; fail if it exists |
| `b` | Binary mode modifier |
| `+` | Enable both reading and writing |

### Work with JSON

```python
import json

data = {
    "name": "Amit",
    "skills": ["Python", "NumPy", "Pandas"]
}

with open("data.json", "w", encoding="utf-8") as file:
    json.dump(data, file, indent=4)

with open("data.json", "r", encoding="utf-8") as file:
    loaded_data = json.load(file)
```

The `with` statement ensures that the file is properly closed.

## 19. Modules and Packages

### Import modules

```python
import math
import random
from datetime import datetime

print(math.sqrt(25))
print(math.pi)
print(datetime.now())
```

### Import specific functions

```python
from math import sqrt, ceil, floor

sqrt(16)
ceil(3.2)
floor(3.8)
```

### Create a module

Create `calculator.py`:

```python
def add(a, b):
    return a + b
```

Use it in another file:

```python
from calculator import add

print(add(10, 20))
```

### Main guard

```python
def main():
    print("Program started")

if __name__ == "__main__":
    main()
```

This allows a file to be run directly without automatically executing the `main()` function when imported.

### Example project structure

```text
project/
├── main.py
├── calculator.py
├── requirements.txt
└── utils/
    ├── __init__.py
    └── helpers.py
```

A package organizes related modules. An `__init__.py` file is commonly used to mark a regular package, although namespace packages can work without it.

## 20. Object-Oriented Programming

### Create a class

```python
class Student:
    def __init__(self, name, score):
        self.name = name
        self.score = score

    def display(self):
        return f"{self.name}: {self.score}"

student = Student("Amit", 90)
print(student.display())
```

### Instance and class attributes

```python
class Employee:
    company = "CubicNova"

    def __init__(self, name):
        self.name = name
```

`company` is a class attribute; `name` is an instance attribute.

### Inheritance

```python
class Animal:
    def speak(self):
        return "Animal sound"

class Dog(Animal):
    def speak(self):
        return "Bark"

dog = Dog()
print(dog.speak())
```

### Encapsulation

Python uses naming conventions rather than strict access modifiers for many forms of encapsulation.

```python
class Account:
    def __init__(self, balance):
        self._balance = balance

    @property
    def balance(self):
        return self._balance
```

A leading underscore signals that an attribute is intended for internal use.

### Class methods and static methods

```python
class Calculator:
    @staticmethod
    def add(a, b):
        return a + b

    @classmethod
    def description(cls):
        return f"Class: {cls.__name__}"
```

### Dataclasses

```python
from dataclasses import dataclass

@dataclass
class Product:
    name: str
    price: float

product = Product("Notebook", 50.0)
print(product)
```

Dataclasses reduce boilerplate for classes primarily used to store data.

## 21. Iterators and Generators

### Iterator

```python
numbers = [10, 20, 30]

iterator = iter(numbers)

print(next(iterator))
print(next(iterator))
print(next(iterator))
```

Calling `next()` after exhaustion raises `StopIteration`.

### Generator function

```python
def count_up_to(limit):
    number = 1

    while number <= limit:
        yield number
        number += 1

for value in count_up_to(5):
    print(value)
```

Generators yield values lazily, which can reduce memory consumption for large sequences.

### Generator expression

```python
squares = (x**2 for x in range(1_000_000))

print(next(squares))
```

Use a generator when values can be processed one at a time instead of storing the entire result in memory.

## 22. Decorators and Context Managers

### Decorator

```python
from functools import wraps

def log_call(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        return func(*args, **kwargs)

    return wrapper

@log_call
def greet(name):
    return f"Hello, {name}"

print(greet("Amit"))
```

Decorators wrap functions to add behavior such as logging, timing, caching, or authorization checks.

### Context manager

```python
with open("data.txt", "r", encoding="utf-8") as file:
    content = file.read()
```

Context managers handle setup and cleanup automatically. The `with` statement is particularly useful for files, locks, and database connections.

## 23. Dates and Times

### Current date and time

```python
from datetime import date, datetime, timedelta

today = date.today()
now = datetime.now()

print(today)
print(now)
```

### Date arithmetic

```python
today = date.today()
next_week = today + timedelta(days=7)
yesterday = today - timedelta(days=1)
```

### Format and parse dates

```python
now = datetime.now()

formatted = now.strftime("%Y-%m-%d %H:%M:%S")
parsed = datetime.strptime("2026-10-18", "%Y-%m-%d")
```

For applications that work across time zones, prefer timezone-aware datetimes rather than assuming local time.

## 24. Regular Expressions

Use the `re` module for pattern matching in text.

```python
import re

text = "Contact us at support@example.com"

match = re.search(r"\w+@\w+\.\w+", text)

if match:
    print(match.group())
```

### Common methods

```python
re.search(pattern, text)
re.match(pattern, text)
re.findall(pattern, text)
re.sub(pattern, replacement, text)
re.split(pattern, text)
```

### Common patterns

| Pattern | Meaning |
|---|---|
| `\d` | Digit |
| `\w` | Word character |
| `\s` | Whitespace |
| `.` | Any character except newline by default |
| `^` | Start of string or line, depending on flags |
| `$` | End of string or line, depending on flags |
| `+` | One or more repetitions |
| `*` | Zero or more repetitions |
| `{n}` | Exactly n repetitions |
| `[A-Z]` | Uppercase ASCII letter |

Use raw strings such as `r"\d+"` for regular expression patterns.

## 25. Virtual Environments and pip

### Create a virtual environment

```bash
python -m venv .venv
```

### Activate on Windows

Command Prompt:

```bash
.venv\Scripts\activate
```

PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

### Activate on Linux or macOS

```bash
source .venv/bin/activate
```

### Install and manage packages

```bash
python -m pip install numpy pandas
python -m pip list
python -m pip freeze > requirements.txt
python -m pip install -r requirements.txt
```

Deactivate:

```bash
deactivate
```

Virtual environments isolate project dependencies. Avoid committing the environment directory to Git; include a dependency file instead.

## 26. Python for Data Science

Python is the foundation of many data science workflows.

### NumPy: Numerical computing

```python
import numpy as np

data = np.array([10, 20, 30, 40])

print(data.mean())
print(data.max())
```

### Pandas: Data analysis

```python
import pandas as pd

df = pd.DataFrame({
    "name": ["Amit", "Priya", "Rahul"],
    "score": [85, 95, 78]
})

print(df.head())
print(df["score"].mean())
```

### Matplotlib: Visualization

```python
import matplotlib.pyplot as plt

x = [1, 2, 3, 4]
y = [10, 20, 15, 30]

plt.plot(x, y, marker="o")
plt.xlabel("Observation")
plt.ylabel("Value")
plt.title("Example Trend")
plt.show()
```

### Scikit-learn: Machine learning

```python
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression

X = [[1], [2], [3], [4], [5]]
y = [2, 4, 6, 8, 10]

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

model = LinearRegression()
model.fit(X_train, y_train)

predictions = model.predict(X_test)
print(predictions)
```

For real projects, split data appropriately and fit preprocessing steps only on the training data to avoid data leakage.

### Useful Python libraries

| Library | Primary use |
|---|---|
| NumPy | Numerical arrays and mathematical operations |
| Pandas | Tabular data manipulation |
| Matplotlib | Data visualization

