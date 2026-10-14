# Reference Solutions — 10 Exception Handling

These are sample answers; other clear and correct implementations are possible.

```python
# 1: Syntax errors prevent parsing; exceptions happen during execution.

# 2
def parse_integer(text):
    try:
        return int(text)
    except ValueError:
        return None  # Use only if this fallback is documented.

# 3
def divide(a, b):
    if b == 0:
        raise ZeroDivisionError("denominator must not be zero")
    return a / b

# 4
try:
    value = int("42")
except ValueError:
    print("Invalid integer")
else:
    print("Parsed:", value)

# 5
try:
    print("Performing work")
finally:
    print("Cleanup")

# 6: A bare except can hide unexpected failures.

# 7
def calculator_divide(a, b):
    try:
        return float(a) / float(b)
    except ValueError:
        return "Inputs must be numeric"
    except ZeroDivisionError:
        return "Denominator cannot be zero"

# 8
def validate_age(age):
    if age < 0:
        raise ValueError("age must be non-negative")
    return age

# 9
def safe_index(items, index):
    try:
        return items[index]
    except IndexError as exc:
        raise IndexError("requested index is outside the sequence") from exc

# 10
def lookup(mapping, key):
    try:
        return mapping[key]
    except KeyError:
        return "Key not found"

# 11
from pathlib import Path
def read_file(path):
    try:
        return Path(path).read_text(encoding="utf-8")
    except FileNotFoundError:
        return None

# 12: finally is useful for cleanup; context managers are often better.

# 13
class InsufficientFundsError(Exception):
    pass

# 14
def withdraw(balance, amount):
    if amount <= 0:
        raise ValueError("amount must be positive")
    if amount > balance:
        raise InsufficientFundsError("insufficient funds")
    return balance - amount

# 15
def parse_score(text):
    try:
        return int(text)
    except ValueError as exc:
        raise ValueError("score must be an integer") from exc

# 16: raise signals an error; return supplies a normal result.

# 17
def require_length(value):
    try:
        return len(value)
    except TypeError as exc:
        raise TypeError("value must support len()") from exc

# 18: KeyError indicates a missing mapping key.
# 19: IndexError indicates an out-of-range sequence index.

# 20
def validate_percentage(value):
    if not 0 <= value <= 100:
        raise ValueError("percentage must be between 0 and 100")
    return value

# 21
import logging
try:
    int("bad")
except ValueError:
    logging.exception("Could not parse integer")

# 22: Never include credentials or tokens in logs.

# 23
try:
    number = float("3.5")
except ValueError:
    print("Invalid number")
else:
    print("Parsed:", number)

# 24
import json
def parse_json(text):
    try:
        return json.loads(text)
    except json.JSONDecodeError:
        return None

# 25
def read_report(path):
    try:
        return Path(path).read_text(encoding="utf-8")
    except FileNotFoundError:
        print("Report file was not found.")
        return None

# 26: A broad try may mislabel errors from unrelated code.
# 27: Returning None for every failure loses information.

# 28
class InvalidRecordError(Exception):
    pass

# 29
def average(values):
    if not values:
        raise ValueError("values cannot be empty")
    return sum(values) / len(values)

# 30
def validate_withdrawal_amount(amount):
    if amount <= 0:
        raise ValueError("amount must be positive")
    return amount

# 31
try:
    int("bad")
except ValueError as exc:
    raise ValueError("expected an integer value") from exc

# 32
file = open("notes.txt", encoding="utf-8")
try:
    content = file.read()
finally:
    file.close()

# 33: with open(path, encoding="utf-8") as file: ... closes automatically.
# 34: Test valid division, division by zero, and invalid argument types.
# 35: Catch specific exceptions, keep try blocks narrow, and avoid silent failures.
```

## Notes
- Use `None` as a fallback only when it is part of the documented contract.
- Prefer clear exceptions when callers need to distinguish failure from valid output.
- Use context managers for files and other supported resources.
