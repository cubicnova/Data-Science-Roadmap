# 07 – Functions in Python

## Overview
A **function** is a reusable block of code that performs a task. Functions organize programs, reduce repetition, and make code easier to test and maintain.

## Learning objectives
By the end of this module, you should be able to:
- Define and call functions using `def`.
- Use parameters, arguments, return values, and defaults.
- Work with keyword arguments, `*args`, and `**kwargs`.
- Understand local and global scope.
- Use docstrings, type hints, lambda functions, and recursion.
- Create reusable functions for data-processing tasks.

## Prerequisites
Variables and data types, operators, conditional statements, loops, and basic data structures.

## 1. Defining and calling functions
```python
def greet():
    print("Hello, learner!")

greet()
```
`def` starts a function definition. The indented body runs when the function is called.

## 2. Parameters and arguments
Parameters are names in the definition; arguments are values supplied in a call.
```python
def greet_person(name):
    print(f"Hello, {name}!")

greet_person("Asha")
```

## 3. Return values versus printing
`print()` displays a value. `return` sends a result back to the caller so it can be reused.
```python
def square(number):
    return number ** 2

result = square(6)
print(result)  # 36
```
A function without an explicit `return` returns `None`.

## 4. Multiple parameters and default arguments
```python
def power(base, exponent=2):
    return base ** exponent

power(4)                    # 16
power(2, 3)                 # 8
power(base=3, exponent=4)   # 81
```
Required parameters should come before parameters with default values.

## 5. Keyword-only and positional-only parameters
`/` marks positional-only parameters; `*` marks keyword-only parameters.
```python
def divide(a, b, /, *, precision=2):
    return round(a / b, precision)

divide(7, 3, precision=3)
```

## 6. `*args` and `**kwargs`
`*args` collects extra positional arguments into a tuple. `**kwargs` collects extra keyword arguments into a dictionary.
```python
def total(*numbers):
    return sum(numbers)

def describe_user(**details):
    return details

total(1, 2, 3)
describe_user(name="Asha", role="analyst")
```

## 7. Scope
Variables created inside a function are normally local. Python resolves names using LEGB: Local, Enclosing, Global, Built-in. Prefer returning values rather than modifying globals.
```python
def make_message(name):
    message = f"Welcome, {name}"
    return message
```

## 8. Docstrings and type hints
Docstrings describe a function. Type hints communicate expected types but are not automatically enforced at runtime.
```python
def celsius_to_fahrenheit(celsius: float) -> float:
    """Convert Celsius to Fahrenheit."""
    return celsius * 9 / 5 + 32
```

## 9. Lambda functions
A lambda is a short anonymous function containing one expression.
```python
double = lambda value: value * 2
double(5)  # 10
```
Use named functions for complex logic. Lambdas are useful as short sorting keys:
```python
students = [("Ravi", 78), ("Mina", 92), ("Dev", 85)]
sorted(students, key=lambda student: student[1])
```

## 10. Functions as values and higher-order functions
Functions can be assigned to variables, passed as arguments, and returned by other functions.
```python
def apply_operation(value, operation):
    return operation(value)

apply_operation(-5, abs)
```

## 11. Recursion
A recursive function calls itself and needs a base case.
```python
def factorial(n):
    if n < 0:
        raise ValueError("n must be non-negative")
    if n <= 1:
        return 1
    return n * factorial(n - 1)
```
For very large inputs, iteration may be safer because Python limits recursion depth.

## 12. Practical data-science examples
```python
def clean_names(names):
    return [name.strip() for name in names if name.strip()]

def mean(values):
    if not values:
        raise ValueError("values cannot be empty")
    return sum(values) / len(values)
```

## Common mistakes
- Defining a function but forgetting to call it.
- Confusing `print()` with `return`.
- Forgetting to return a value on every required branch.
- Placing a default parameter before a required parameter.
- Using a mutable default such as `items=[]`; prefer `None` and create a new list inside.
- Accessing a local variable outside its scope.
- Writing recursion without a base case.
- Making one function perform unrelated tasks.

## Best practices
- Use descriptive `snake_case` names.
- Keep each function focused on one task.
- Return results when callers need to reuse them.
- Add docstrings for reusable functions.
- Validate inputs and test edge cases.
- Minimize side effects.

## Quick reference

| Concept | Example |
|---|---|
| Define | `def add(a, b):` |
| Call | `add(2, 3)` |
| Return | `return a + b` |
| Default | `def greet(name="Guest"):` |
| Keyword argument | `greet(name="Asha")` |
| Variable positional args | `def f(*args):` |
| Variable keyword args | `def f(**kwargs):` |
| Lambda | `lambda x: x * 2` |
| Type hint | `def f(x: int) -> int:` |

## Module files
- `README.md` — theory, syntax, and examples
- `practice.md` — 35 exercises
- `examples.ipynb` — runnable notebook examples
- `solutions.md` — reference answers
