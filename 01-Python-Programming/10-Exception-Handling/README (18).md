# 10 – Exception Handling in Python

## Overview
Exception handling allows a program to respond to runtime errors in a controlled way. It can report a problem, recover where appropriate, or stop safely while preserving useful diagnostic information.

## Learning objectives
- Distinguish syntax errors from runtime exceptions.
- Use `try`, `except`, `else`, and `finally`.
- Catch specific exceptions.
- Raise built-in and custom exceptions.
- Use exception chaining.
- Validate input and design reliable functions.
- Avoid common exception-handling anti-patterns.

## 1. Errors and exceptions
A **syntax error** prevents Python from parsing a program. An **exception** occurs while code is running, such as dividing by zero.

```python
print(10 / 0)  # raises ZeroDivisionError
```

## 2. Basic `try` and `except`
```python
try:
    number = int("42")
except ValueError:
    print("Please provide a valid integer.")
else:
    print(number)
```
Catch the narrowest useful exception. `ValueError` is appropriate when text cannot be parsed as an integer.

## 3. Multiple exception handlers
```python
def divide_text(a_text, b_text):
    try:
        a = float(a_text)
        b = float(b_text)
        return a / b
    except ValueError:
        return "Both inputs must be numbers."
    except ZeroDivisionError:
        return "The denominator must not be zero."
```
Keep handlers meaningful; avoid catching errors you cannot handle or report usefully.

## 4. The `else` clause
The `else` block runs only if the `try` block finishes without an exception. Put success-only code there so it is not accidentally covered by the handler.

## 5. The `finally` clause
The `finally` block runs whether or not an exception occurs and is commonly used for cleanup. For files and other supported resources, prefer context managers (`with`) where available.

## 6. Raising exceptions
Use `raise` when a function cannot proceed with invalid input.
```python
def square_root_input(number):
    if number < 0:
        raise ValueError("number must be non-negative")
    return number ** 0.5
```
Choose an exception type that accurately describes the problem.

## 7. Custom exceptions
```python
class InsufficientFundsError(Exception):
    pass

def withdraw(balance, amount):
    if amount <= 0:
        raise ValueError("amount must be positive")
    if amount > balance:
        raise InsufficientFundsError("balance is too low")
    return balance - amount
```
Custom exceptions generally inherit from `Exception`, not directly from `BaseException`.

## 8. Exception chaining
When translating a lower-level error into a clearer one, preserve the original cause with `raise ... from ...`.
```python
def parse_score(text):
    try:
        return int(text)
    except ValueError as exc:
        raise ValueError("score must be an integer") from exc
```

## 9. Common built-in exceptions

| Exception | Typical cause |
|---|---|
| `ValueError` | Correct type, invalid value |
| `TypeError` | Unsuitable type for an operation |
| `ZeroDivisionError` | Division by zero |
| `IndexError` | Sequence index out of range |
| `KeyError` | Missing dictionary key |
| `FileNotFoundError` | File does not exist |
| `PermissionError` | Operation is not permitted |
| `AttributeError` | Attribute does not exist |
| `ImportError` | Import cannot be completed |
| `TimeoutError` | Operation timed out |

## 10. Input validation
Validate data at boundaries such as user input, file parsing, APIs, and function arguments.
```python
def percentage(part, whole):
    if whole == 0:
        raise ZeroDivisionError("whole must not be zero")
    if part < 0 or whole < 0:
        raise ValueError("values must be non-negative")
    return part / whole * 100
```

## 11. Anti-patterns to avoid
Avoid silently swallowing errors:
```python
try:
    risky_operation()
except:
    pass
```
This hides failures and makes debugging difficult. Also avoid broad `try` blocks, misleading fallback values, and logging secrets or sensitive data.

## 12. Logging exceptions
```python
import logging
try:
    int("not a number")
except ValueError:
    logging.exception("Could not parse the supplied integer")
```
Call `logging.exception()` from an exception handler. It includes traceback details; never log passwords or tokens.

## Best practices
- Catch specific exceptions.
- Keep `try` blocks as small as practical.
- Use `else` for success-only work and `finally` for cleanup.
- Raise clear, accurate exceptions for invalid inputs.
- Preserve causes when translating errors.
- Use context managers for resources.
- Do not silently swallow exceptions.
- Test successful and failure paths.

## Quick reference

| Purpose | Syntax |
|---|---|
| Handle failure | `try: ... except ValueError: ...` |
| Run on success | `else: ...` |
| Cleanup | `finally: ...` |
| Raise error | `raise ValueError("message")` |
| Custom error | `class MyError(Exception): ...` |
| Chain cause | `raise MyError("message") from exc` |
| Log traceback | `logging.exception("message")` |

## Module files
- `README.md` — theory, syntax, and examples
- `practice.md` — 35 exercises
- `examples.ipynb` — runnable notebook examples
- `solutions.md` — reference answers
