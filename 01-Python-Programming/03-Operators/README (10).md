# Operators in Python

## Overview

Operators are symbols or keywords that perform operations on values and variables. They are essential for calculations, comparisons, logical decisions, and data manipulation in Python.

This module covers Python's main operator families:

- Arithmetic operators
- Comparison (relational) operators
- Assignment and augmented assignment operators
- Logical operators
- Bitwise operators
- Membership operators
- Identity operators
- Operator precedence and associativity
- Practical use of operators in data science

## Learning Objectives

By the end of this module, learners should be able to:

1. Use arithmetic operators to perform numeric calculations.
2. Compare values and interpret Boolean results.
3. Update variables using assignment and augmented assignment.
4. Combine conditions using `and`, `or`, and `not`.
5. Explain and use bitwise operators on integers.
6. Check membership with `in` and `not in`.
7. Distinguish object identity (`is`) from value equality (`==`).
8. Apply operator precedence and parentheses correctly.
9. Use operators in data validation, feature calculations, and simple analytical tasks.

## 1. Arithmetic Operators

Arithmetic operators perform mathematical operations.

| Operator | Meaning | Example | Result |
|---|---|---|---:|
| `+` | Addition | `8 + 3` | `11` |
| `-` | Subtraction | `8 - 3` | `5` |
| `*` | Multiplication | `8 * 3` | `24` |
| `/` | True division | `8 / 3` | `2.666...` |
| `//` | Floor division | `8 // 3` | `2` |
| `%` | Modulus/remainder | `8 % 3` | `2` |
| `**` | Exponentiation | `2 ** 3` | `8` |

```python
price = 250
quantity = 4
subtotal = price * quantity
print(subtotal)  # 1000

total_seconds = 367
minutes = total_seconds // 60
seconds = total_seconds % 60
print(minutes, seconds)  # 6 7
```

**Important:** `/` returns a floating-point result, while `//` floors the quotient. For negative values, floor division rounds down toward negative infinity: `-7 // 3 == -3`.

## 2. Comparison Operators

Comparison operators compare two values and return `True` or `False`.

| Operator | Meaning | Example |
|---|---|---|
| `==` | Equal to | `5 == 5` → `True` |
| `!=` | Not equal to | `5 != 3` → `True` |
| `>` | Greater than | `5 > 3` → `True` |
| `<` | Less than | `3 < 5` → `True` |
| `>=` | Greater than or equal to | `5 >= 5` → `True` |
| `<=` | Less than or equal to | `3 <= 5` → `True` |

```python
score = 78
print(score >= 40)          # True
print(score == 100)         # False
print(0 <= score <= 100)    # True
```

Python supports chained comparisons such as `0 <= score <= 100`, which is generally clearer than repeating `score`.

## 3. Assignment Operators

The assignment operator `=` binds a name to a value. Augmented assignment operators combine an operation with reassignment.

| Operator | Equivalent form | Example |
|---|---|---|
| `=` | Assign | `x = 10` |
| `+=` | `x = x + value` | `x += 2` |
| `-=` | `x = x - value` | `x -= 2` |
| `*=` | `x = x * value` | `x *= 2` |
| `/=` | `x = x / value` | `x /= 2` |
| `//=` | `x = x // value` | `x //= 2` |
| `%=` | `x = x % value` | `x %= 2` |
| `**=` | `x = x ** value` | `x **= 2` |

```python
balance = 100
balance += 25
balance -= 10
print(balance)  # 115
```

Assignment is not the same as comparison: `x = 5` assigns a value; `x == 5` tests equality.

## 4. Logical Operators

Logical operators combine or invert Boolean expressions.

- `and`: true when both operands are truthy.
- `or`: true when at least one operand is truthy.
- `not`: reverses the truth value.

```python
age = 22
has_id = True
print(age >= 18 and has_id)  # True
print(age < 18 or has_id)    # True
print(not has_id)            # False
```

`and` and `or` short-circuit: Python may not evaluate the right-hand expression if the result is already determined. They return one of their operands, not necessarily a Boolean. In conditions, they are commonly used with Boolean expressions.

```python
items = []
if items and items[0] == "admin":
    print("First item is admin")
else:
    print("List is empty or first item is not admin")
```

This pattern avoids indexing an empty list.

## 5. Bitwise Operators

Bitwise operators work on the binary representation of integers.

| Operator | Meaning | Example | Result |
|---|---|---|---:|
| `&` | Bitwise AND | `6 & 3` | `2` |
| `\|` | Bitwise OR | `6 \| 3` | `7` |
| `^` | Bitwise XOR | `6 ^ 3` | `5` |
| `~` | Bitwise NOT | `~6` | `-7` |
| `<<` | Left shift | `3 << 1` | `6` |
| `>>` | Right shift | `8 >> 1` | `4` |

For positive integers, shifting left by one bit often multiplies by two, and shifting right by one bit often performs floor division by two. Bitwise operations are useful in flags, low-level programming, and some security/networking tasks; they are not substitutes for logical `and`/`or`.

```python
READ = 0b001
WRITE = 0b010
EXECUTE = 0b100

permissions = READ | WRITE
can_write = (permissions & WRITE) != 0
print(permissions)  # 3
print(can_write)    # True
```

## 6. Membership Operators

Membership operators test whether a value appears in a sequence or collection.

- `in`
- `not in`

```python
tools = ["Python", "Pandas", "NumPy"]
print("Pandas" in tools)       # True
print("R" not in tools)        # True
print("data" in "data science") # True
```

For dictionaries, membership checks keys by default:

```python
record = {"name": "Asha", "score": 91}
print("score" in record)  # True
print(91 in record)       # False
```

## 7. Identity Operators

Identity operators check whether two references point to the same object.

- `is`
- `is not`

Use `==` to compare values. Use `is` most commonly to compare with singletons such as `None`.

```python
first = [1, 2]
second = [1, 2]
third = first

print(first == second)  # True: same contents
print(first is second)  # False: distinct list objects
print(first is third)   # True: same object
```

```python
result = None
if result is None:
    print("No result available")
```

Do not use `is` as a general replacement for `==`.

## 8. Operator Precedence

Precedence determines which operations are evaluated first. Parentheses make intent explicit.

A simplified order from higher to lower precedence:

1. Parentheses and indexing/calls
2. Exponentiation `**`
3. Unary operators such as `+x`, `-x`, `~x`
4. Multiplication, division, floor division, modulus: `*`, `/`, `//`, `%`
5. Addition and subtraction: `+`, `-`
6. Shifts: `<<`, `>>`
7. Bitwise AND, XOR, OR: `&`, `^`, `|`
8. Comparisons, membership, identity: `<`, `==`, `in`, `is`, etc.
9. Logical `not`
10. Logical `and`
11. Logical `or`

Assignment has its own rules and is not an ordinary expression operator in the same way. Exponentiation is right-associative: `2 ** 3 ** 2` means `2 ** (3 ** 2)`.

```python
print(2 + 3 * 4)      # 14
print((2 + 3) * 4)    # 20
print(2 ** 3 ** 2)    # 512
```

When readability matters, use parentheses even when precedence would make them unnecessary.

## 9. Truthiness

Values such as `0`, `0.0`, `""`, `[]`, `{}`, `set()`, and `None` are false in Boolean contexts. Most other values are true.

```python
username = ""
if not username:
    print("Username is missing")
```

Truthiness is useful for simple checks, but use explicit checks when the distinction matters. For example, `if value is None` distinguishes `None` from `0`.

## 10. Operators in Data Science

Operators are used throughout data analysis and machine learning:

- Calculate totals, averages, percentages, and derived features.
- Filter records with comparison and logical operators.
- Check whether categories or labels are in an allowed set.
- Create Boolean masks for data selection.
- Combine conditions for data quality rules.

```python
scores = [82, 47, 35, 91, 68]
passing = [score for score in scores if score >= 40]
above_average_threshold = [score for score in scores if score >= 70]
print(passing)                 # [82, 47, 91, 68]
print(above_average_threshold) # [82, 91]
```

With libraries such as NumPy and pandas, comparisons can create arrays or Series of Boolean values. Logical combination of array conditions often uses `&` and `|` with parentheses rather than Python's scalar `and` and `or`.

## Common Mistakes

1. Using `=` where `==` is required.
2. Using `is` to compare ordinary values instead of `==`.
3. Confusing `/` with `//`.
4. Forgetting operator precedence in mixed expressions.
5. Using `and` with array-like NumPy/pandas objects instead of elementwise operators.
6. Expecting `in dictionary` to check values rather than keys.
7. Forgetting that `%` returns the remainder and can be useful for parity checks such as `number % 2 == 0`.

## Quick Reference

```python
# Arithmetic
total = 10 + 5 * 2

# Comparison
is_valid = 0 <= total <= 100

# Logical
eligible = is_valid and total >= 20

# Membership
allowed = "Python" in ["Python", "R", "SQL"]

# Identity
missing = None
is_missing = missing is None

# Modulus: even/odd
is_even = total % 2 == 0
```

## Completion Checklist

- [ ] I can use arithmetic operators and explain `/`, `//`, and `%`.
- [ ] I can compare values and interpret Boolean results.
- [ ] I can update variables with augmented assignment.
- [ ] I can combine conditions using `and`, `or`, and `not`.
- [ ] I can explain the difference between bitwise and logical operators.
- [ ] I can use membership and identity operators correctly.
- [ ] I can apply parentheses to clarify precedence.
- [ ] I can use operators to calculate and filter simple data.
