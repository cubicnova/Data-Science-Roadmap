# Data Structures in Python

## Overview
Data structures organize and store data so that programs can access, update, search, and process it efficiently. Python provides built-in collection types that are foundational for data analysis and machine learning.

This module covers:
- Lists
- Tuples
- Sets
- Dictionaries
- Strings as sequences
- Indexing, slicing, and iteration
- Mutable versus immutable objects
- Common collection methods
- Nested collections
- Choosing the right data structure
- Practical data-processing patterns

## Learning Objectives
By the end of this module, you should be able to:
1. Select a suitable Python collection for a task.
2. Create, access, update, and remove elements from lists, tuples, sets, and dictionaries.
3. Use indexing and slicing with sequences.
4. Explain mutability and basic implications for variables.
5. Iterate over collections and nested structures.
6. Use sets for uniqueness and membership checks.
7. Use dictionaries for key-value lookups and frequency counts.
8. Apply collection types to simple data-science tasks.

## 1. Lists
A list is an ordered, mutable collection. It allows duplicate values and can contain mixed data types.

```python
scores = [82, 91, 75, 91]
print(scores[0])      # 82
print(scores[-1])     # 91
scores[1] = 95
scores.append(88)
scores.remove(75)
print(scores)         # [82, 95, 91, 88]
```

Common methods:
- `append(value)`: add one item to the end.
- `extend(iterable)`: add items from another iterable.
- `insert(index, value)`: insert at an index.
- `remove(value)`: remove the first matching value; raises `ValueError` if absent.
- `pop(index)`: remove and return an item; defaults to the last item.
- `sort()` / `reverse()`: modify the list in place.
- `index(value)` / `count(value)`: locate or count matching values.

### Indexing and slicing
Indexes start at zero; negative indexes count from the end. The slice stop is excluded.

```python
numbers = [10, 20, 30, 40, 50]
print(numbers[1:4])  # [20, 30, 40]
print(numbers[:3])   # [10, 20, 30]
print(numbers[::2])  # [10, 30, 50]
```

## 2. Tuples
A tuple is ordered and immutable. It is useful for fixed collections and returning multiple values.

```python
coordinates = (10.5, 20.2)
print(coordinates[0])
# coordinates[0] = 15  # TypeError: tuple items cannot be reassigned
```

A one-item tuple requires a trailing comma: `single = (5,)`. Parentheses are often optional when creating tuples, but they improve readability.

## 3. Sets
A set is an unordered collection of unique hashable elements. It is useful for removing duplicates and testing membership.

```python
values = [1, 2, 2, 3, 3, 3]
unique_values = set(values)
print(unique_values)  # {1, 2, 3} (display order may vary)

a = {1, 2, 3}
b = {3, 4, 5}
print(a | b)  # union
print(a & b)  # intersection
print(a - b)  # difference
print(a ^ b)  # symmetric difference
```

Use `set()` to create an empty set; `{}` creates an empty dictionary. Set elements must be hashable, so lists and dictionaries cannot be set elements.

## 4. Dictionaries
A dictionary stores key-value pairs. Keys must be hashable and are unique; values can be any object.

```python
student = {"name": "Asha", "score": 91}
print(student["name"])
student["score"] = 95
student["city"] = "Indore"
print(student.get("email", "Not provided"))
```

Common methods:
- `get(key, default)`: safely read a key with a fallback.
- `keys()`, `values()`, `items()`: view keys, values, or key-value pairs.
- `update(other)`: merge or update entries.
- `pop(key)`: remove and return a value.
- `setdefault(key, default)`: get a value, inserting a default if missing.

```python
for key, value in student.items():
    print(key, value)
```

Use `key in dictionary` to test whether a key exists. Use `value in dictionary.values()` to test values.

## 5. Strings as Sequences
Strings are ordered and immutable sequences of Unicode characters.

```python
text = "Data Science"
print(text[0])       # D
print(text[0:4])     # Data
print(text.lower())
print(text.replace("Science", "Analysis"))
```

String methods such as `split()`, `strip()`, `lower()`, `upper()`, and `replace()` return new strings rather than modifying the original string.

## 6. Mutable and Immutable Objects
Mutable objects can be changed after creation; immutable objects cannot have their contents changed in place.

- Common mutable types: `list`, `dict`, `set`.
- Common immutable types: `int`, `float`, `bool`, `str`, `tuple` (though a tuple can contain mutable objects).

```python
first = [1, 2]
second = first
second.append(3)
print(first)  # [1, 2, 3]: both names refer to the same list

original = [1, 2]
copy = original.copy()
copy.append(3)
print(original)  # [1, 2]
print(copy)      # [1, 2, 3]
```

A shallow copy duplicates the outer collection but does not recursively copy nested objects.

## 7. Nested Data Structures
Collections can contain other collections. Nested dictionaries and lists are common when working with JSON-like records.

```python
students = [
    {"name": "Asha", "marks": [88, 91, 85]},
    {"name": "Ravi", "marks": [78, 82, 80]},
]
print(students[0]["name"])
print(students[0]["marks"][1])
```

Always check that keys or indexes exist when data may be incomplete or irregular.

## 8. Choosing a Data Structure

| Type | Ordered? | Mutable? | Duplicates? | Typical use |
|---|---|---|---|---|
| `list` | Yes | Yes | Yes | Editable sequence |
| `tuple` | Yes | No | Yes | Fixed record or grouping |
| `set` | No guaranteed positional order | Yes | No | Uniqueness and set operations |
| `dict` | Insertion-ordered | Yes | Keys are unique | Key-value lookup |
| `str` | Yes | No | Characters can repeat | Text processing |

Choose based on the operations you need. Lists are convenient for sequences; sets for uniqueness and membership; dictionaries for lookups by key; tuples for fixed groups of values.

## 9. Common Data Processing Patterns

### Remove duplicates while retaining original order
```python
values = [3, 1, 3, 2, 1]
unique_in_order = list(dict.fromkeys(values))
print(unique_in_order)  # [3, 1, 2]
```

### Count frequencies with a dictionary
```python
words = ["data", "python", "data", "analysis"]
frequencies = {}
for word in words:
    frequencies[word] = frequencies.get(word, 0) + 1
print(frequencies)
```

### Group records by a category
```python
records = [
    {"name": "Asha", "team": "Blue"},
    {"name": "Ravi", "team": "Red"},
    {"name": "Mina", "team": "Blue"},
]
grouped = {}
for record in records:
    team = record["team"]
    grouped.setdefault(team, []).append(record["name"])
print(grouped)
```

## 10. Data Structures in Data Science
- Lists store small datasets and intermediate results.
- Dictionaries represent records, configurations, and JSON data.
- Sets help identify unique categories and compare groups.
- Tuples represent fixed-size records or coordinates.
- Strings store labels, categories, and raw text.

Libraries such as NumPy and pandas provide specialized arrays and tables for larger analytical workloads, but Python's built-in structures are essential for understanding data organization and control flow.

## Common Mistakes
1. Accessing a list index that does not exist.
2. Assuming list slicing includes the stop index.
3. Trying to modify a tuple or string in place.
4. Using `{}` when an empty set is intended.
5. Expecting a set to preserve a useful positional order.
6. Assuming `dict[key]` is safe when the key may be absent; consider `.get()`.
7. Removing items from a list while iterating over the same list.
8. Forgetting that `list_a = list_b` does not copy the list.
9. Trying to put unhashable values such as lists into a set or use them as dictionary keys.
10. Assuming a shallow copy also copies nested lists or dictionaries.

## Completion Checklist
- [ ] I can create and update lists, tuples, sets, and dictionaries.
- [ ] I can use indexing and slicing.
- [ ] I understand mutability and copying.
- [ ] I can perform set operations and membership tests.
- [ ] I can safely access dictionary values.
- [ ] I can work with nested collections.
- [ ] I can choose an appropriate data structure for a task.
- [ ] I can use collections for simple data-analysis patterns.
