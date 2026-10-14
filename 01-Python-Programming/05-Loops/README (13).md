# Loops in Python

## Overview
Loops repeat a block of code. They help automate repetitive tasks, process collections, aggregate data, and implement iterative algorithms.

This module covers:
- `for` loops and iterables
- `while` loops and loop conditions
- `range()`
- `break`, `continue`, and `pass`
- Nested loops
- Looping through strings, lists, tuples, dictionaries, and sets
- Accumulators and counters
- `enumerate()` and `zip()`
- Loop `else` clauses
- Common loop errors and data-science applications

## Learning Objectives
By the end of this module, you should be able to:
1. Choose between `for` and `while`.
2. Iterate over sequences and dictionaries.
3. Use `range()` to generate numeric sequences.
4. Control loop execution with `break` and `continue`.
5. Build nested loops and avoid accidental infinite loops.
6. Use counters and accumulators to summarize data.
7. Use `enumerate()` and `zip()` for readable iteration.
8. Apply loops to simple data processing tasks.

## 1. The `for` Loop
A `for` loop visits items from an iterable, such as a list, string, tuple, set, dictionary, or range.

```python
languages = ["Python", "SQL", "R"]
for language in languages:
    print(language)
```

The loop variable receives one item at a time. The loop ends when the iterable is exhausted.

### Looping through strings
```python
for character in "data":
    print(character)
```

### Looping through dictionaries
Iterating over a dictionary directly gives its keys. Use `.items()` for key-value pairs:

```python
student = {"name": "Asha", "score": 91}
for key, value in student.items():
    print(key, value)
```

## 2. The `range()` Function
`range()` generates a sequence of integers. The stop value is excluded.

- `range(stop)` starts at 0.
- `range(start, stop)` starts at `start`.
- `range(start, stop, step)` increments by `step`.

```python
print(list(range(5)))       # [0, 1, 2, 3, 4]
print(list(range(2, 7)))    # [2, 3, 4, 5, 6]
print(list(range(10, 0, -2)))  # [10, 8, 6, 4, 2]
```

## 3. The `while` Loop
A `while` loop repeats as long as its condition is true. Update the state that controls the condition so the loop can eventually stop.

```python
count = 1
while count <= 5:
    print(count)
    count += 1
```

An infinite loop can occur if the condition never becomes false. Make sure loop variables are updated and termination conditions are correct.

## 4. `break` and `continue`
- `break` immediately exits the nearest enclosing loop.
- `continue` skips the rest of the current iteration and proceeds to the next one.

```python
for number in range(1, 8):
    if number == 5:
        break
    print(number)  # 1, 2, 3, 4
```

```python
for number in range(1, 6):
    if number == 3:
        continue
    print(number)  # 1, 2, 4, 5
```

## 5. `pass`
`pass` does nothing. It is a placeholder when Python syntax requires a statement.

```python
for item in [1, 2, 3]:
    pass  # Placeholder for future work
```

Unlike `break`, `pass` does not exit the loop. Unlike `continue`, it does not skip to the next iteration early.

## 6. Nested Loops
A nested loop is a loop inside another loop. The inner loop runs completely for each outer-loop iteration.

```python
for row in range(1, 4):
    for column in range(1, 3):
        print(row, column)
```

Nested loops are useful for small grids and pairwise combinations, but their runtime can grow quickly. Two loops each running `n` times often perform roughly `n × n` operations.

## 7. Counters and Accumulators
A counter tracks how many times something occurs. An accumulator builds a total.

```python
numbers = [3, 7, 2, 8]
total = 0
for number in numbers:
    total += number
print(total)  # 20
```

```python
numbers = [3, 7, 2, 8]
even_count = 0
for number in numbers:
    if number % 2 == 0:
        even_count += 1
print(even_count)  # 2
```

## 8. `enumerate()` and `zip()`
Use `enumerate()` to get an index and item together. Use `zip()` to iterate over corresponding elements from multiple iterables. By default, `zip()` stops when the shortest iterable ends.

```python
names = ["Asha", "Ravi", "Mina"]
for index, name in enumerate(names, start=1):
    print(index, name)
```

```python
names = ["Asha", "Ravi"]
scores = [91, 85]
for name, score in zip(names, scores):
    print(name, score)
```

## 9. Loop `else`
A loop's `else` block runs if the loop finishes normally, but not if it ends with `break`.

```python
target = 7
for number in [2, 4, 6, 8]:
    if number == target:
        print("Found")
        break
else:
    print("Not found")
```

This pattern can be useful for search tasks. The `else` belongs to the loop, not to an `if`.

## 10. Common Iteration Patterns
### Filter items
```python
scores = [82, 47, 35, 91, 68]
passing = []
for score in scores:
    if score >= 40:
        passing.append(score)
print(passing)
```

### Transform items
```python
numbers = [1, 2, 3, 4]
squares = []
for number in numbers:
    squares.append(number ** 2)
print(squares)  # [1, 4, 9, 16]
```

### Find a maximum manually
```python
values = [8, 3, 12, 5]
largest = values[0]
for value in values[1:]:
    if value > largest:
        largest = value
print(largest)  # 12
```

For empty lists, handle the empty case before reading `values[0]`.

## 11. Loops in Data Science
Loops can help with:
- Counting records that meet a condition
- Calculating totals and averages
- Cleaning or transforming small collections
- Processing rows or batches
- Applying a simple rule-based classification

For large numerical datasets, vectorized operations in NumPy or pandas are often faster and more concise than Python loops. Learn loops first to understand iteration and algorithm logic.

## Common Mistakes
1. Forgetting to update a `while` loop's controlling variable.
2. Using the wrong range endpoint; `range()` excludes `stop`.
3. Accidentally modifying a collection while iterating over it.
4. Confusing `break`, `continue`, and `pass`.
5. Assuming `zip()` includes unmatched items from the longer iterable.
6. Using an uninitialized accumulator.
7. Accessing the first item of an empty list.
8. Indenting statements at the wrong level.
9. Creating unnecessarily expensive nested loops.

## Quick Reference
```python
for item in iterable:
    process(item)

for number in range(1, 6):
    print(number)

while condition:
    update_state()

for item in iterable:
    if should_stop(item):
        break
    if should_skip(item):
        continue
    process(item)
```

## Completion Checklist
- [ ] I can use `for`, `while`, and `range()`.
- [ ] I can iterate over common Python collections.
- [ ] I understand `break`, `continue`, and `pass`.
- [ ] I can build nested loops.
- [ ] I can use counters and accumulators.
- [ ] I can use `enumerate()` and `zip()`.
- [ ] I can avoid infinite loops and empty-list errors.
- [ ] I can use loops to summarize and transform data.
