# Conditional Statements in Python

## Overview
Conditional statements let a program make decisions by running different blocks of code depending on whether a condition is true or false.

Topics: `if`, `if...else`, `if...elif...else`, nested conditions, logical operators, truthiness, conditional expressions, `pass`, input validation, and data-science applications.

## Learning Objectives
By the end of this module, you should be able to:
1. Write conditions using comparison and logical operators.
2. Use `if`, `else`, and `elif` to control program flow.
3. Build nested decisions and simplify them where appropriate.
4. Understand truthiness and explicitly check `None`.
5. Use conditional expressions for simple choices.
6. Validate input and classify data with conditional rules.

## 1. The `if` Statement
An `if` statement executes its indented block only when its condition is true.

```python
temperature = 32
if temperature > 30:
    print("It is hot today.")
```

Syntax:
```python
if condition:
    # statements run when condition is True
```
Python uses indentation to define a block. Four spaces per indentation level is the common convention.

## 2. The `if...else` Statement
Use `else` when one of two paths should be selected.

```python
number = 7
if number % 2 == 0:
    print("Even")
else:
    print("Odd")
```

The `else` block runs when the `if` condition is false.

## 3. The `if...elif...else` Chain
Use `elif` (“else if”) to check multiple conditions in order. The first true branch runs.

```python
score = 84
if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
elif score >= 60:
    grade = "D"
else:
    grade = "Needs improvement"
print(grade)  # B
```

Order matters: put higher thresholds or more specific conditions first. Validate a score's range before assigning a grade if values outside 0–100 are possible.

## 4. Nested `if` Statements
A nested conditional is an `if` inside another `if`. The inner decision is considered only when the outer condition is true.

```python
age = 22
has_ticket = True
if age >= 18:
    if has_ticket:
        print("Entry permitted")
    else:
        print("Ticket required")
else:
    print("Must be at least 18")
```

Avoid excessive nesting when a combined condition or early return would be clearer.

## 5. Combining Conditions
- `and`: every condition must be true.
- `or`: at least one condition must be true.
- `not`: reverses a truth value.

```python
age = 25
has_id = True
if age >= 18 and has_id:
    print("Requirements met")

day = "Sunday"
if day == "Saturday" or day == "Sunday":
    print("Weekend")

score = 76
if 0 <= score <= 100:
    print("Score is in the valid range")
```

Python supports chained comparisons such as `0 <= score <= 100`.

## 6. Truthiness and Explicit Conditions
Values such as `False`, `None`, numeric zero, and empty strings or collections (`""`, `[]`, `{}`) are false in conditions. Most other values are true.

```python
username = ""
if not username:
    print("Username is missing")
```

Use explicit checks when values have different meanings:
```python
value = 0
if value is None:
    print("No value was supplied")
elif value == 0:
    print("The value is zero")
```

Do not treat `None`, `0`, and `""` as interchangeable.

## 7. Conditional Expressions
A conditional expression selects one of two values in one expression.

```python
age = 17
status = "Adult" if age >= 18 else "Minor"
print(status)  # Minor
```

General syntax: `value_if_true if condition else value_if_false`. Use it for simple choices; use a regular conditional chain for more complex logic.

## 8. The `pass` Statement
`pass` is a placeholder that does nothing.

```python
score = 85
if score >= 50:
    pass  # Add success-handling code later
else:
    print("Try again")
```

When the true branch runs, `pass` performs no operation and execution continues after the conditional.

## 9. Input Validation
`input()` returns a string. Convert it before numeric comparisons and handle invalid input.

```python
raw_age = input("Enter your age: ")
try:
    age = int(raw_age)
except ValueError:
    print("Please enter a whole number.")
else:
    if age < 0:
        print("Age cannot be negative.")
    elif age < 18:
        print("Minor")
    else:
        print("Adult")
```

Consider both conversion errors and values outside the allowed range.

## 10. Conditionals in Data Science
Conditionals can classify observations, validate values, and support rule-based data cleaning.

```python
scores = [92, 71, 38, 84, 55]
for score in scores:
    if score >= 80:
        category = "High"
    elif score >= 50:
        category = "Medium"
    else:
        category = "Low"
    print(score, category)
```

For large datasets, pandas and NumPy provide vectorized conditional tools, but the underlying decision logic is similar.

## Common Mistakes
1. Forgetting the colon after `if`, `elif`, or `else`.
2. Using inconsistent indentation.
3. Writing `=` instead of `==` in a comparison.
4. Using separate `if` statements when an `elif` chain is intended.
5. Putting broad conditions before specific conditions.
6. Comparing text input directly with a number without conversion.
7. Assuming `input()` returns an integer.
8. Confusing `None`, zero, and an empty string.

## Quick Reference
```python
if condition:
    do_something()

if condition:
    do_a()
else:
    do_b()

if condition_a:
    do_a()
elif condition_b:
    do_b()
else:
    do_default()

result = "yes" if condition else "no"
```

## Completion Checklist
- [ ] I can use `if`, `if...else`, and `if...elif...else`.
- [ ] I can write nested and combined conditions.
- [ ] I understand truthiness and `None` checks.
- [ ] I can write a conditional expression.
- [ ] I can validate input safely.
- [ ] I can classify values using conditional rules.
