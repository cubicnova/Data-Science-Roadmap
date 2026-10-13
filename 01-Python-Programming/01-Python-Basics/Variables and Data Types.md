# Python Variables and Data Types

Welcome to **Variables and Data Types**, a foundational module in the Python Programming section of the Data Science Roadmap.

This module introduces the concepts used to store, represent, inspect, convert, and manipulate information in Python. These concepts are prerequisites for data structures, functions, NumPy, Pandas, and machine learning.

## Learning Objectives

By completing this module, you will be able to:

- Explain variables, assignment, identifiers, and Python's dynamic typing.
- Follow Python identifier naming rules and conventions.
- Recognize Python's built-in data types.
- Inspect values with `type()` and test types with `isinstance()`.
- Convert values between compatible data types.
- Work with strings, lists, tuples, sets, and dictionaries.
- Explain object mutability and variable references.
- Use multiple assignment, unpacking, and f-strings.
- Validate input and handle common conversion errors.
- Apply variables and data types to small data-analysis tasks.

## Prerequisites

- Basic computer literacy.
- Python 3 installed, or access to Google Colab.
- Jupyter Notebook, JupyterLab, or VS Code is recommended.

## 1. Variables and Assignment

A variable is a name that refers to an object. Assignment binds a name to a value.

```python
name = "Rahul"
age = 21
height = 1.75
is_student = True

print(name, age, height, is_student)
```

Python uses **dynamic typing**: a name can later refer to an object of a different type.

```python
value = 100
print(type(value).__name__)  # int

value = "Python"
print(type(value).__name__)  # str
```

### Identifier rules

- An identifier can contain letters, digits, and underscores.
- It cannot begin with a digit.
- It cannot contain spaces or hyphens.
- Python keywords such as `class`, `if`, and `for` cannot be used as identifiers.
- Names are case-sensitive: `score` and `Score` are different names.

Use descriptive `snake_case` names for variables and functions.

```python
student_name = "Amit"
total_marks = 450
average_score = 90.0
```

## 2. Built-in Data Types

| Type | Purpose | Example |
|---|---|---|
| `int` | Integers | `25` |
| `float` | Floating-point numbers | `3.14` |
| `complex` | Complex numbers | `2 + 3j` |
| `str` | Text | `"Python"` |
| `bool` | Truth values | `True`, `False` |
| `list` | Ordered, mutable sequence | `[10, 20, 30]` |
| `tuple` | Ordered, immutable sequence | `(10, 20, 30)` |
| `set` | Collection of unique hashable values | `{10, 20, 30}` |
| `dict` | Key-value mapping | `{"name": "Amit"}` |
| `NoneType` | Absence of a value | `None` |

Use `type()` to inspect a value. Use `isinstance(value, SomeType)` when checking whether an object is an instance of a type.

```python
items = [42, 3.5, 2 + 1j, "data", False, None]
for item in items:
    print(repr(item), type(item).__name__)
```

## 3. Numeric Types and Expressions

Python's common numeric types are `int`, `float`, and `complex`.

```python
price = 250.0
quantity = 4
total = price * quantity
print(f"Total cost: {total:.2f}")
```

Common arithmetic operators include `+`, `-`, `*`, `/`, `//`, `%`, and `**`. Division with `/` produces a floating-point result. Check for a zero divisor before division.

## 4. Strings and Formatting

Strings are immutable sequences of Unicode characters. They can be created with single or double quotes.

```python
first_name = "Priya"
last_name = "Sharma"
full_name = f"{first_name} {last_name}"

print(full_name)
print(full_name.upper())
print(full_name.lower())
print(len(full_name))
```

Use `strip()` to remove leading and trailing whitespace and f-strings to format output.

```python
product = "Notebook"
quantity = 3
unit_price = 49.5
print(f"{product}: {quantity} × ₹{unit_price:.2f} = ₹{quantity * unit_price:.2f}")
```

## 5. Boolean Values

Boolean values are `True` and `False`. Comparisons and logical expressions produce Boolean results.

```python
age = 20
has_permission = True
eligible = age >= 18 and has_permission
print(eligible)
```

In Python, `0`, `None`, and empty strings or collections are falsy. Most other values are truthy; for example, `"0"` and `[0]` are truthy because they are non-empty.

## 6. Collection Types

### Lists

Lists are ordered and mutable.

```python
marks = [75, 80, 92]
marks.append(88)
marks[0] = 77
print(marks)
```

### Tuples

Tuples are ordered and their item references cannot be reassigned.

```python
coordinates = (10, 20)
print(coordinates[0])
```

A tuple can contain a mutable object, and that contained object may still be mutated.

### Sets

Sets store unique hashable values. Set iteration order is not guaranteed.

```python
languages = {"Python", "SQL", "Python"}
print(languages)  # contains Python and SQL
```

### Dictionaries

Dictionaries map unique keys to values.

```python
student = {"name": "Amit", "marks": 85}
student["department"] = "Computer Science"
student["marks"] = 90
print(student)
```

## 7. Type Conversion

Functions such as `int()`, `float()`, `str()`, `list()`, `tuple()`, and `set()` perform conversions when supported. Conversion can raise exceptions such as `ValueError` or `TypeError`.

```python
age = int("21")
price = float("99.50")
label = str(100)

print(age + 1, price, label)
```

`input()` always returns a string. Convert it before doing numeric calculations.

```python
try:
    amount = float(input("Enter an amount: "))
    print(f"Amount: {amount:.2f}")
except ValueError:
    print("Please enter a valid number.")
```

## 8. Mutability and References

Mutable objects can be changed in place; immutable objects cannot. A variable is a name, not a container that owns a separate copy of every value.

```python
first = [1, 2, 3]
second = first
second.append(4)

print(first)          # [1, 2, 3]
print(first is second)  # True
```

To make a shallow copy of a list, use `first.copy()` or `first[:]`. A shallow copy does not recursively copy nested objects.

Common mutable types include `list`, `dict`, and `set`. Common immutable types include `int`, `float`, `bool`, `str`, and `tuple` (as a container).

## 9. Multiple Assignment and Unpacking

```python
x, y, z = 10, 20, 30
x, z = z, x
print(x, y, z)
```

Unpacking requires the number of target names to match the number of values, unless starred unpacking is used.

## 10. Constants and Naming Conventions

Python does not enforce ordinary constants. By convention, uppercase names represent values intended to remain unchanged.

```python
PI = 3.14159
MAX_ATTEMPTS = 3
```

## 11. Application to Data Science

Variables and types are used to represent measurements, labels, observations, configuration values, and results. Lists and dictionaries are useful for introductory exercises; NumPy arrays and Pandas DataFrames are introduced in later modules.

```python
student = {
    "name": "Neha",
    "marks": [80, 90, 85],
    "is_enrolled": True,
}

marks = student["marks"]
average = sum(marks) / len(marks)

print(f"Student: {student['name']}")
print(f"Average marks: {average:.2f}")
```

## Practice and Notebook

- Complete the exercises in [`practice.md`](practice.md).
- Run and modify the examples in [`examples.ipynb`](examples.ipynb).
- Check your work with [`solutions.md`](solutions.md).

## Further Learning

- [Official Python Tutorial](https://docs.python.org/3/tutorial/)
- [Python Built-in Types](https://docs.python.org/3/library/stdtypes.html)
- [Python Built-in Functions](https://docs.python.org/3/library/functions.html)

## Next Topic

**Operators and Expressions in Python** — arithmetic, comparison, logical, assignment, identity, and membership operators.
