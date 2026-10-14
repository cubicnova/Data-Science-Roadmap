# Reference Solutions — 07 Functions

These are sample answers; other correct implementations are possible.

```python
# 1
def greet():
    print("Hello, Python!")

# 2
def show_name(name):
    print(name)

# 3
def add(a, b):
    return a + b

# 4
def subtract(a, b):
    return a - b

# 5
def square(n):
    return n ** 2

# 6
def is_even(n):
    return n % 2 == 0

# 7
def maximum(a, b):
    return a if a >= b else b

# 8
def greet(name="Guest"):
    return f"Hello, {name}!"

# 9
def full_name(first, last):
    return f"{first} {last}"

# 10
def area_rectangle(width, height):
    return width * height

# 11
def celsius_to_fahrenheit(c):
    return c * 9 / 5 + 32

# 12
def count_vowels(text):
    return sum(ch.lower() in "aeiou" for ch in text)

# 13
def reverse_text(text):
    return text[::-1]

# 14
def factorial(n):
    if n < 0:
        raise ValueError("n must be non-negative")
    result = 1
    for value in range(2, n + 1):
        result *= value
    return result

# 15
def average(numbers):
    if not numbers:
        raise ValueError("numbers cannot be empty")
    return sum(numbers) / len(numbers)

# 16
def describe_person(name, age=18):
    return f"{name} is {age} years old."

# 17
def sum_all(*numbers):
    return sum(numbers)

# 18
def make_profile(**details):
    return details

# 19
def apply_twice(value, function):
    return function(function(value))

# 20
triple = lambda x: x * 3

# 21
def sort_by_score(records):
    return sorted(records, key=lambda row: row[1])

# 22
def remove_empty(strings):
    return [item.strip() for item in strings if item.strip()]

# 23
def safe_divide(a, b):
    if b == 0:
        raise ZeroDivisionError("b cannot be zero")
    return a / b

# 24
def is_palindrome(text):
    normalized = "".join(text.lower().split())
    return normalized == normalized[::-1]

# 25
def min_max(numbers):
    if not numbers:
        raise ValueError("numbers cannot be empty")
    return min(numbers), max(numbers)

# 26
def percent(part, whole):
    if whole == 0:
        raise ValueError("whole cannot be zero")
    return part / whole * 100

# 27
def format_currency(amount, symbol="₹"):
    return f"{symbol}{amount:,.2f}"

# 28
def filter_positive(numbers):
    return [n for n in numbers if n > 0]

# 29
def median(numbers):
    if not numbers:
        raise ValueError("numbers cannot be empty")
    ordered = sorted(numbers)
    middle = len(ordered) // 2
    if len(ordered) % 2:
        return ordered[middle]
    return (ordered[middle - 1] + ordered[middle]) / 2

# 30
def sum_to_n(n):
    if n < 0:
        raise ValueError("n must be non-negative")
    if n == 0:
        return 0
    return n + sum_to_n(n - 1)

# 31
def make_multiplier(factor):
    def multiplier(value):
        return value * factor
    return multiplier

# 32
def clean_columns(columns):
    return [column.strip().lower() for column in columns]

# 33
def unique_in_order(items):
    seen = set()
    result = []
    for item in items:
        if item not in seen:
            seen.add(item)
            result.append(item)
    return result

# 34
def calculate_stats(values):
    if not values:
        raise ValueError("values cannot be empty")
    return {
        "count": len(values),
        "total": sum(values),
        "mean": sum(values) / len(values),
    }

# 35
def validate_password(password):
    return len(password) >= 8 and any(ch.isdigit() for ch in password)
```

## Notes
- Exercises 1 and 2 explicitly request printing.
- Returning values usually makes functions easier to test.
- Validate empty sequences and zero divisors explicitly.
- `unique_in_order` assumes hashable items because it uses a set.
