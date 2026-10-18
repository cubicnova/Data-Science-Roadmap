# Python Interview Practice — Solutions

These are reference solutions, not the only valid answers. Try to explain each one in your own words and test edge cases.

## Part A — Conceptual answer guide

1. **List:** ordered and mutable; useful for a sequence you will update. **Tuple:** ordered and immutable; useful for fixed records. **Set:** unique hashable elements; useful for membership and deduplication. **Dictionary:** key-value mapping; useful for lookup and counting.
2. Mutable objects can change in place (for example, `list`, `dict`, `set`). Immutable objects cannot be changed after creation (for example, `int`, `float`, `bool`, `str`, `tuple`, `frozenset`).
3. `==` compares values; `is` checks whether two references point to the same object. Use `is None` and `is not None`; do not use `is` for ordinary value comparisons.
4. A shallow copy creates a new outer container but keeps references to nested objects. A deep copy recursively copies nested objects, subject to the object's copying behavior.
5. Defaults are evaluated once when the function is defined. Use `None` as a sentinel, then create a fresh list inside the function.
6. `*args` collects extra positional arguments into a tuple; `**kwargs` collects extra keyword arguments into a dictionary.
7. An iterable can produce an iterator; an iterator tracks iteration state and provides `__next__`; a generator is a convenient iterator created by a generator function or expression and yields lazily.
8. `try` wraps risky code; `except` handles matching exceptions; `else` runs if no exception occurred in `try`; `finally` runs during cleanup regardless of whether an exception occurred.
9. Instance methods receive `self`; class methods receive `cls` and are marked with `@classmethod`; static methods receive no implicit instance or class and are marked with `@staticmethod`.
10. A decorator wraps or transforms a function/class, often for logging, timing, caching, authorization, or retries.
11. A context manager coordinates setup/cleanup. `with open(...)` closes the file even if an exception occurs.
12. A list comprehension concisely transforms/filters an iterable. Prefer a loop when logic is complex, nested, or benefits from debugging and comments.
13. `list.sort()` mutates a list and returns `None`; `sorted(iterable)` returns a new sorted list and accepts any iterable.
14. Hashable objects have a stable hash during their lifetime and equality behavior suitable for hash-table keys. Dictionary keys must be hashable; lists and dictionaries are not.
15. LEGB means Local, Enclosing, Global, Built-in: the name lookup order for typical function scopes.
16. `break` exits the nearest loop; `continue` skips to the next iteration; `pass` is a no-op placeholder.
17. Dynamic typing means names are bound to objects at runtime and are not permanently declared with one type. Type hints can document/check expectations but are not enforced by the interpreter by default.
18. The GIL historically limits simultaneous execution of Python bytecode by threads in conventional CPython builds, but implementation and build matter; modern CPython has optional free-threaded builds. Do not claim all Python implementations or configurations behave identically.
19. Syntax errors prevent code from being parsed; exceptions occur during execution and may be handled.
20. Measure first with representative inputs, using tools such as `timeit` for microbenchmarks and `cProfile` for whole-program profiling. Identify a bottleneck, record baseline and workload, then optimize and re-measure.

## Part B — Predict the output

1. `[1, 2, 3]` — `b` and `a` refer to the same list.
2. `([1, 2, 3], 'x')` — the tuple cannot replace its first element, but that element is a mutable list.
3. First call returns `['a']`; second returns `['a', 'b']` — the same default list is reused.
4. Prints `[1, 4, 9]`, then `n is not a global` — the comprehension variable does not become a global name in Python 3.
5. Prints `[1, 2, 3] None` — `sort()` mutates in place and returns `None`.
6. Prints `fallback` and `7` — `or` and `and` return operands and short-circuit based on truthiness.

## Part C — Coding solutions

```python
from collections import Counter, defaultdict
from math import isfinite


def reverse_string(text):
    return text[::-1]


def is_palindrome(text):
    cleaned = "".join(char.casefold() for char in text if char.isalnum())
    return cleaned == cleaned[::-1]


def character_counts(text):
    # Spaces and punctuation count as characters.
    return dict(Counter(text))


def unique_preserving_order(items):
    seen = set()
    result = []
    for item in items:
        if item not in seen:
            seen.add(item)
            result.append(item)
    return result


def second_largest(numbers):
    distinct = set(numbers)
    if len(distinct) < 2:
        raise ValueError("At least two distinct values are required")
    return sorted(distinct)[-2]


def two_sum(numbers, target):
    seen = {}
    for index, value in enumerate(numbers):
        complement = target - value
        if complement in seen:
            return [seen[complement], index]
        seen[value] = index
    raise ValueError("No pair sums to target")


def are_anagrams(left, right):
    normalize = lambda text: sorted(char.casefold() for char in text if not char.isspace())
    return normalize(left) == normalize(right)


def flatten_one_level(nested):
    return [item for group in nested for item in group]


def group_by_first_letter(words):
    groups = defaultdict(list)
    for word in words:
        if word:
            groups[word[0].casefold()].append(word)
    return dict(groups)


def merge_sum_dicts(left, right):
    result = dict(left)
    for key, value in right.items():
        result[key] = result.get(key, 0) + value
    return result


def top_k_frequent(items, k):
    if k < 0:
        raise ValueError("k must be non-negative")
    counts = Counter(items)
    # Stable deterministic tie-break: first appearance in input.
    first_seen = {}
    for index, item in enumerate(items):
        first_seen.setdefault(item, index)
    return sorted(counts, key=lambda item: (-counts[item], first_seen[item]))[:k]


def chunks(items, size):
    if size <= 0:
        raise ValueError("size must be positive")
    for start in range(0, len(items), size):
        yield items[start:start + size]


def moving_average(values, window):
    if window <= 0:
        raise ValueError("window must be positive")
    if window > len(values):
        return []
    result = []
    running_sum = sum(values[:window])
    result.append(running_sum / window)
    for end in range(window, len(values)):
        running_sum += values[end] - values[end - window]
        result.append(running_sum / window)
    return result


def get_nested(data, keys, default=None):
    current = data
    for key in keys:
        if not isinstance(current, dict) or key not in current:
            return default
        current = current[key]
    return current


def total_by_team(records):
    totals = defaultdict(float)
    for record in records:
        if "team" not in record or "score" not in record:
            raise ValueError("Every record must contain 'team' and 'score'")
        team, score = record["team"], record["score"]
        if not isinstance(team, str) or not team:
            raise ValueError("'team' must be a non-empty string")
        if isinstance(score, bool) or not isinstance(score, (int, float)):
            raise ValueError("'score' must be numeric")
        if not isfinite(score):
            raise ValueError("'score' must be finite")
        totals[team] += score
    return dict(totals)


def running_mean(values):
    total = 0.0
    count = 0
    for value in values:
        if isinstance(value, bool) or not isinstance(value, (int, float)):
            raise TypeError("values must be numeric")
        total += value
        count += 1
        yield total / count


def validate_row(row):
    errors = []
    name = row.get("name")
    age = row.get("age")
    if not isinstance(name, str) or not name.strip():
        errors.append("'name' must be a non-empty string")
    if isinstance(age, bool) or not isinstance(age, int) or age < 0:
        errors.append("'age' must be a non-negative integer")
    return errors
```

### Complexity notes

- `reverse_string`: \(O(n)\) time and \(O(n)\) output space.
- `is_palindrome`: \(O(n)\) time and \(O(n)\) cleaned-string space.
- `character_counts`: \(O(n)\) expected time and \(O(u)\) space for `u` distinct characters.
- `unique_preserving_order`: expected \(O(n)\) time and \(O(u)\) space, assuming hashable items.
- `second_largest`: expected \(O(n)\) to build the set plus \(O(u \log u)\) to sort distinct values. A one-pass solution can achieve expected \(O(n)\).
- `two_sum`: expected \(O(n)\) time and \(O(n)\) space.
- `are_anagrams`: \(O(n \log n + m \log m)\) due to sorting.
- `flatten_one_level`: \(O(n)\) time and \(O(n)\) output space.
- `group_by_first_letter`: \(O(n)\) overall relative to total input characters, plus output storage.
- `merge_sum_dicts`: expected \(O(n + m)\) time and \(O(n + m)\) output space.
- `top_k_frequent`: \(O(n + u \log u)\) time and \(O(u)\) space; a heap can help when `k` is much smaller than `u`.
- `chunks`: yields slices; total copying is \(O(n)\) for list input and holds each yielded chunk at a time, excluding consumer storage.
- `moving_average`: \(O(n)\) time and \(O(n)\) output space.
- `get_nested`: \(O(d)\) time for path depth `d`.
- `total_by_team`: expected \(O(n)\) time and \(O(t)\) space for `t` teams.
- `running_mean`: \(O(n)\) time and \(O(1)\) auxiliary state, excluding the consumer's stored results.
- `validate_row`: \(O(1)\) time and space for this fixed schema.

## Part D — Debugging

### 1. Mutable default
```python
def collect(value, result=None):
    if result is None:
        result = []
    result.append(value)
    return result
```
Each call without `result` now creates a fresh list.

### 2. Mutating a list while iterating
Create a new list:
```python
numbers = [1, 2, 3, 4, 5, 6]
numbers = [number for number in numbers if number % 2 != 0]
```
Removing elements while iterating shifts later elements and can cause some to be skipped.

### 3. Empty average
```python
def average(values):
    if not values:
        raise ValueError("average requires at least one value")
    return sum(values) / len(values)
```
An empty input has no defined arithmetic mean; make the contract explicit.

### 4. Shadowing built-ins
Use `numbers = [1, 2, 3]`, not `list = ...`. Shadowing makes `list(...)` unavailable in that scope and confuses readers.

### 5. Profiling
Use realistic inputs and measure a baseline. For a small isolated function, `timeit` can help; for a whole application, `cProfile` can identify where runtime is spent. Check algorithmic complexity, repeated work, I/O, and memory. Change one thing at a time and re-measure.

## Suggested testing examples

```python
assert reverse_string("python") == "nohtyp"
assert is_palindrome("A man, a plan, a canal: Panama")
assert character_counts("aba") == {"a": 2, "b": 1}
assert unique_preserving_order([1, 2, 1, 3]) == [1, 2, 3]
assert second_largest([4, 1, 4, 3]) == 3
assert two_sum([2, 7, 11, 15], 9) == [0, 1]
assert are_anagrams("Dormitory", "dirty room")
assert flatten_one_level([[1, 2], [3], [], [4]]) == [1, 2, 3, 4]
assert merge_sum_dicts({"a": 2, "b": 1}, {"a": 3, "c": 4}) == {"a": 5, "b": 1, "c": 4}
assert moving_average([1, 2, 3, 4], 2) == [1.5, 2.5, 3.5]
assert get_nested({"a": {"b": 3}}, ["a", "b"]) == 3
assert list(running_mean([2, 4, 6])) == [2.0, 3.0, 4.0]
assert validate_row({"name": "Ada", "age": 36}) == []
```
