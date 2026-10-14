# Python Comprehensions

Comprehensions provide a concise way to create lists, sets, and dictionaries from iterable data. They can replace many simple loops while keeping the transformation close to the result being built.

## Learning objectives

By the end of this module, you should be able to:
- Write list, set, and dictionary comprehensions.
- Use conditions to filter values.
- Transform items with expressions.
- Work with nested comprehensions.
- Choose between a comprehension, a loop, and a generator expression.

## 1. List comprehensions

**General syntax**

```python
[expression for item in iterable]
```

Example:

```python
squares = [number ** 2 for number in range(1, 6)]
print(squares)  # [1, 4, 9, 16, 25]
```

Equivalent loop:

```python
squares = []
for number in range(1, 6):
    squares.append(number ** 2)
```

## 2. Filtering with `if`

```python
evens = [number for number in range(10) if number % 2 == 0]
print(evens)  # [0, 2, 4, 6, 8]
```

The `if` at the end filters which items are included.

## 3. Conditional expressions

Use `if ... else` inside the expression when every input should produce a result.

```python
labels = ["even" if number % 2 == 0 else "odd" for number in range(5)]
print(labels)  # ['even', 'odd', 'even', 'odd', 'even']
```

Distinguish these two patterns:

```python
# Filter: include only even numbers
[number for number in range(6) if number % 2 == 0]

# Transform every number into a label
["even" if number % 2 == 0 else "odd" for number in range(6)]
```

## 4. String comprehensions

Strings are iterable, so comprehensions can transform their characters.

```python
letters = [character.upper() for character in "python"]
print(letters)  # ['P', 'Y', 'T', 'H', 'O', 'N']
```

## 5. Set comprehensions

Set comprehensions use curly braces and produce unique values.

```python
unique_lengths = {len(word) for word in ["cat", "dog", "python", "owl"]}
print(unique_lengths)  # {3, 6}
```

Sets do not preserve a meaningful display order. Sort the result if a stable order is required.

## 6. Dictionary comprehensions

Dictionary comprehensions create key-value pairs.

```python
squares = {number: number ** 2 for number in range(1, 5)}
print(squares)  # {1: 1, 2: 4, 3: 9, 4: 16}
```

You can transform existing mappings:

```python
prices = {"pen": 10, "book": 80}
with_tax = {item: price * 1.05 for item, price in prices.items()}
```

If two generated entries have the same key, the later value replaces the earlier one.

## 7. Nested comprehensions

A nested comprehension can flatten or transform nested data.

```python
matrix = [[1, 2], [3, 4], [5, 6]]
flat = [value for row in matrix for value in row]
print(flat)  # [1, 2, 3, 4, 5, 6]
```

The order of `for` clauses follows the order of nested loops:

```python
pairs = [(x, y) for x in [1, 2] for y in ["a", "b"]]
```

For complex nested logic, ordinary loops may be easier to read.

## 8. Generator expressions

Generator expressions use parentheses and produce values lazily, rather than creating the full list immediately.

```python
squares = (number ** 2 for number in range(1, 6))
print(sum(squares))  # 55
```

Use a list comprehension when you need a reusable list. Use a generator expression when values can be consumed one at a time, especially for large data.

## 9. Common patterns

```python
# Strip whitespace from strings
cleaned = [name.strip() for name in [" Ada ", " Grace ", "Linus"]]

# Normalize case and filter empty strings
names = [name.strip().title() for name in [" ada ", "", "grace "] if name.strip()]

# Keep only positive values
positive = [value for value in [-3, 0, 5, 8] if value > 0]

# Dictionary lookup / mapping
lengths = {word: len(word) for word in ["red", "green", "blue"]}
```

## 10. Best practices

- Prefer comprehensions for straightforward transformations and filters.
- Use descriptive variable names that make the expression easy to scan.
- Avoid deeply nested or overly long comprehensions; split them into steps or use loops.
- Do not use comprehensions solely for side effects such as printing or modifying external state.
- Remember that set results are unique and dictionary keys must be unique.
- Consider generator expressions when the full result does not need to be stored.
- Test edge cases, including empty input and unexpected values.

## Quick reference

| Type | Syntax | Result |
|---|---|---|
| List | `[expr for item in iterable]` | List |
| Filtered list | `[expr for item in iterable if condition]` | List |
| Set | `{expr for item in iterable}` | Set of unique values |
| Dictionary | `{key: value for item in iterable}` | Dictionary |
| Generator | `(expr for item in iterable)` | Lazy iterator |

## Summary

Comprehensions make common data transformations concise. Start with list comprehensions, add filters and conditional expressions, then learn set and dictionary comprehensions. Use nested comprehensions only when they remain readable.
