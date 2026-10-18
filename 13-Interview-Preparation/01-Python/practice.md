# Python Interview Practice

Try the questions before opening `solution.md`. For coding exercises, include a short explanation, complexity, and tests.

## Part A — Conceptual questions

1. What is the difference between a list, tuple, set, and dictionary? Give a use case for each.
2. Explain mutable vs immutable objects. Which common built-in types are mutable?
3. Explain `==` versus `is`. When should you use `is`?
4. What is the difference between a shallow copy and a deep copy?
5. Why can a mutable default argument be dangerous? How should it be handled?
6. What are `*args` and `**kwargs`?
7. What is the difference between an iterable, iterator, and generator?
8. What do `try`, `except`, `else`, and `finally` do?
9. Explain instance methods, class methods, and static methods.
10. What is a decorator? Name a practical use.
11. What is a context manager, and why is `with open(...)` useful?
12. Explain list comprehensions and when a regular loop may be clearer.
13. What is the difference between `sort()` and `sorted()`?
14. What is a hashable object? Why must dictionary keys be hashable?
15. Explain LEGB scope resolution.
16. What is the difference between `break`, `continue`, and `pass`?
17. What does it mean that Python uses dynamic typing?
18. What is the Global Interpreter Lock (GIL)? Answer with implementation/version context rather than an absolute claim.
19. What are exceptions versus syntax errors?
20. How would you profile a slow Python function before optimizing it?

## Part B — Predict the output

For each snippet, write the output and explain why.

### 1. Aliasing and mutation
```python
a = [1, 2]
b = a
b.append(3)
print(a)
```

### 2. Tuple containing a mutable object
```python
value = ([1, 2], "x")
value[0].append(3)
print(value)
```

### 3. Default argument
```python
def add_item(item, items=[]):
    items.append(item)
    return items

print(add_item("a"))
print(add_item("b"))
```

### 4. Comprehension scope
```python
numbers = [1, 2, 3]
squares = [n * n for n in numbers]
print(squares)
print(n if "n" in globals() else "n is not a global")
```

### 5. Sorting
```python
values = [3, 1, 2]
result = values.sort()
print(values, result)
```

### 6. Boolean short-circuiting
```python
print([] or "fallback")
print("yes" and 7)
```

## Part C — Coding challenges

### Beginner

1. **Reverse a string:** Write `reverse_string(text)` without using `reversed()`.
2. **Palindrome check:** Write `is_palindrome(text)` that ignores case and non-alphanumeric characters. Examples: `"A man, a plan, a canal: Panama"` → `True`; `"Python"` → `False`.
3. **Count frequencies:** Write `character_counts(text)` returning a dictionary of character frequencies. Decide and document whether spaces count.
4. **Remove duplicates:** Write `unique_preserving_order(items)` that removes duplicates while preserving first occurrence order.
5. **Second-largest distinct number:** Write `second_largest(numbers)`. Raise `ValueError` if there are fewer than two distinct values.

### Intermediate

6. **Two Sum:** Given a list of integers and a target, return indices of two distinct elements whose sum equals the target. Assume exactly one answer exists.
7. **Anagram check:** Write `are_anagrams(left, right)` ignoring spaces and case.
8. **Flatten one level:** Convert `[[1, 2], [3], [], [4, 5]]` into `[1, 2, 3, 4, 5]` without recursion.
9. **Group words by first letter:** Given a list of words, return a dictionary mapping each lowercase first letter to words that start with it. Ignore empty strings.
10. **Merge dictionaries by summing values:** Given `{"a": 2, "b": 1}` and `{"a": 3, "c": 4}`, return `{"a": 5, "b": 1, "c": 4}`.
11. **Top-k frequent items:** Return the `k` most frequent values. State how you handle ties.
12. **Chunk a list:** Write `chunks(items, size)` yielding consecutive lists of at most `size` elements. Reject non-positive sizes.

### Advanced / data-science flavored

13. **Moving average:** Given numeric values and a positive window size, return all simple moving averages. Define behavior when the window is larger than the input.
14. **Safe dictionary lookup:** Implement `get_nested(data, keys, default=None)` to traverse nested dictionaries and return the default if any key is missing or the path is not a dictionary.
15. **Batch records:** Given a list of dictionaries containing `team` and `score`, return total score per team. Treat missing fields as invalid input and explain your choice.
16. **Streaming mean:** Implement a generator that yields the running mean after each numeric value.
17. **Validate a CSV-like row:** Given a row dictionary, validate that `name` is a non-empty string and `age` is a non-negative integer (booleans should not count as integers). Return useful validation errors.

## Part D — Debugging prompts

1. Why does this function return the same list across calls? Fix it.
```python
def collect(value, result=[]):
    result.append(value)
    return result
```

2. This code removes items unexpectedly. Explain and fix it.
```python
numbers = [1, 2, 3, 4, 5, 6]
for number in numbers:
    if number % 2 == 0:
        numbers.remove(number)
```

3. This code fails for empty input. Fix it and explain the edge case.
```python
def average(values):
    return sum(values) / len(values)
```

4. This code can overwrite a built-in. Why is that risky? Rename the variable.
```python
list = [1, 2, 3]
```

5. A function is slow on a large list. Describe how you would measure it and what evidence you would collect before changing the algorithm.

## Part E — Mock interview checklist

For each coding answer, be ready to explain:
- What assumptions did you make?
- What is the simplest correct approach?
- Why did you choose this data structure?
- What are time and auxiliary-space complexities?
- What happens for empty input, duplicates, invalid types, or ties?
- Which tests would catch likely bugs?
- How would you adapt the solution to a large or streaming dataset?
