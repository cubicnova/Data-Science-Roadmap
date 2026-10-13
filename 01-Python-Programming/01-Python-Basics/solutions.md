# Python Variables and Data Types — Reference Solutions

Try each exercise independently before consulting these examples. Many problems have multiple valid solutions. These answers illustrate one readable approach.

## Exercise 1 — Personal Profile

```python
name = "Rahul"
age = 21
city = "Indore"
qualification = "Bachelor's degree"
career_goal = "Data Scientist"

print(f"Name: {name}")
print(f"Age: {age}")
print(f"City: {city}")
print(f"Qualification: {qualification}")
print(f"Career goal: {career_goal}")
```

## Exercise 2 — Multiple Assignment

```python
a, b, c = 10, 20, 30
print(a, b, c)
```

## Exercise 3 — Variable Swapping

```python
a, b = 10, 20
a, b = b, a
print(a, b)  # 20 10
```

## Exercise 4 — Identifier Rules

| Identifier | Valid? | Reason |
|---|---|---|
| `student_name` | Yes | Letters and underscore are allowed. |
| `2students` | No | Cannot begin with a digit. |
| `total_marks` | Yes | Valid identifier. |
| `first-name` | No | Hyphen is not allowed in an identifier. |
| `class` | No | `class` is a Python keyword. |
| `_private` | Yes | A leading underscore is allowed. |
| `value2` | Yes | Digits are allowed after the first character. |

## Exercise 5 — Dynamic Typing

```python
value = 100
print(type(value).__name__)  # int

value = "Python"
print(type(value).__name__)  # str
```

## Exercise 6 — Type Identification

```python
values = [
    10, 3.14, 2 + 3j, "Python", True,
    [1, 2], (1, 2), {1, 2}, {"name": "Amit"}, None
]

for value in values:
    print(repr(value), type(value).__name__)
```

## Exercise 7 — Numeric Operations

```python
a, b = 20, 5

print("Sum:", a + b)
print("Difference:", a - b)
print("Product:", a * b)

if b != 0:
    print("Quotient:", a / b)
else:
    print("Cannot divide by zero.")
```

## Exercise 8 — String Formatting

```python
name, age, city = "Amit", 21, "Indore"
print(f"{name} is {age} years old and lives in {city}.")
```

## Exercise 9 — Collection Creation

```python
subjects = ["Python", "Statistics", "SQL", "Math", "ML"]
coordinates = (10, 20, 30)
numbers = {1, 2, 3, 4, 5}
student = {"name": "Amit", "marks": 85}

print(subjects)
print(coordinates)
print(numbers)
print(student)
```

## Exercise 10 — Unique Values, Preserving Order

```python
values = [10, 20, 10, 30, 20, 40, 30]
unique_values = list(dict.fromkeys(values))
print(unique_values)  # [10, 20, 30, 40]
```

## Exercise 11 — String to Integer

```python
number = int("250")
print(number + 50)  # 300
```

## Exercise 12 — Integer to Float

```python
value = float(100)
print(value)        # 100.0
print(type(value))  # <class 'float'>
```

## Exercise 13 — Numeric Input

```python
try:
    a = float(input("First number: "))
    b = float(input("Second number: "))
    print("Sum:", a + b)
except ValueError:
    print("Please enter valid numeric values.")
```

## Exercise 14 — Conversion Errors

```python
value = "Python"

try:
    print(int(value))
except ValueError:
    print("This text cannot be converted to an integer.")
```

`int("Python")` raises `ValueError` because the text does not represent a valid integer.

## Exercise 15 — Boolean Conversion

```python
values = [0, 1, "", "Python", [], [0], None]

for value in values:
    print(repr(value), "->", bool(value))
```

Expected results: `0`, `""`, `[]`, and `None` are false; `1`, `"Python"`, and `[0]` are true. Empty values are generally falsy, while non-empty strings and collections are truthy.

## Exercise 16 — String Length

```python
sentence = "Data Science with Python"
print(len(sentence))
```

## Exercise 17 — String Manipulation

```python
text = "python programming"
print(text.upper())
print(text.lower())
print(text.title())
```

## Exercise 18 — Full Name

```python
first = input("First name: ").strip()
last = input("Last name: ").strip()
print(f"{first} {last}".strip())
```

## Exercise 19 — Formatted Receipt

```python
product = "Notebook"
quantity = 3
unit_price = 49.50
total = quantity * unit_price

print(f"Product: {product}")
print(f"Quantity: {quantity}")
print(f"Unit price: {unit_price:.2f}")
print(f"Total: {total:.2f}")
```

## Exercise 20 — Data Cleaning

```python
text = "   Python for Data Science   "
print(text.strip())
```

## Exercise 21 — List Modification

```python
numbers = [10, 20, 30, 40, 50]
numbers[0] = 100
numbers.append(60)
numbers.remove(30)
print(numbers)  # [100, 20, 40, 50, 60]
```

## Exercise 22 — Tuple Access

```python
cities = ("Indore", "Mumbai", "Delhi", "Pune", "Jaipur")
print(cities[0])
print(cities[2])
print(cities[-1])
```

Tuples do not support item assignment. Attempting `cities[0] = "Bhopal"` raises `TypeError`.

## Exercise 23 — Dictionary Update

```python
student = {"name": "Amit", "marks": 80}
student["department"] = "Computer Science"
student["marks"] = 90
print(student)
```

## Exercise 24 — Set Operations

```python
a = {"Python", "SQL", "R"}
b = {"Python", "Java", "SQL"}

print("Union:", a | b)
print("Intersection:", a & b)
print("Difference:", a - b)
```

Set display order may vary.

## Exercise 25 — Mutable Objects

```python
first = [10, 20, 30]
second = first
second.append(40)

print(first)            # [10, 20, 30, 40]
print(second)           # [10, 20, 30, 40]
print(first is second)  # True
```

Both names reference the same list object, so mutation through either name is visible through both.

## Exercise 26 — Multiple Unpacking

```python
values = [10, 20, 30]
a, b, c = values
a, c = c, a
print(a, b, c)  # 30 20 10
```

## Exercise 27 — Variable Scope

```python
value = "global"

def show_value():
    value = "local"
    print("Inside:", value)

show_value()
print("Outside:", value)
```

The local name exists inside the function; the global name remains unchanged.

## Exercise 28 — Data Type Inspector

```python
def convert_value(text, choice):
    try:
        if choice == "int":
            return int(text)
        if choice == "float":
            return float(text)
        if choice == "str":
            return text
        raise ValueError("Choose int, float, or str.")
    except ValueError as error:
        return f"Conversion error: {error}"

text = input("Enter a value: ")
choice = input("Interpret as int, float, or str: ").strip().lower()
print(convert_value(text, choice))
```

## Exercise 29 — Student Marks Analyzer

```python
marks = [80, 75, 90, 85, 70]

print("Total:", sum(marks))
print("Average:", sum(marks) / len(marks))
print("Highest:", max(marks))
print("Lowest:", min(marks))
```

This assumes the list contains at least one numeric mark.

## Exercise 30 — Product Price Calculator

```python
product = "Keyboard"
unit_price = 1500.0
quantity = 2
discount_percent = 10.0

if unit_price < 0 or quantity < 0:
    raise ValueError("Price and quantity must be non-negative.")
if not 0 <= discount_percent <= 100:
    raise ValueError("Discount must be between 0 and 100.")

subtotal = unit_price * quantity
discount = subtotal * discount_percent / 100
final_amount = subtotal - discount

print(f"Product: {product}")
print(f"Subtotal: {subtotal:.2f}")
print(f"Discount: {discount:.2f}")
print(f"Final amount: {final_amount:.2f}")
```

## Exercise 31 — Employee Salary Record

```python
employee = {
    "name": "Rahul",
    "employee_id": "EMP101",
    "basic_salary": 30000.0,
    "allowance": 5000.0,
}

gross_salary = employee["basic_salary"] + employee["allowance"]

print("Name:", employee["name"])
print("ID:", employee["employee_id"])
print(f"Gross salary: {gross_salary:.2f}")
```

Assumption: gross salary is basic salary plus allowance; taxes and deductions are not included.

## Exercise 32 — Data Conversion Pipeline

```python
values = ["10", "20", "30", "40", "50"]
numbers = [int(value) for value in values]

average = sum(numbers) / len(numbers)
squares = [number ** 2 for number in numbers]

print(numbers)
print("Average:", average)
print("Squares:", squares)
```

## Exercise 33 — Swap Dictionary Values

```python
record = {"first": 10, "second": 20}
record["first"], record["second"] = record["second"], record["first"]
print(record)
```

## Exercise 34 — Data Validation

```python
def make_student(name, age, marks):
    name = name.strip()

    if not name:
        raise ValueError("Name cannot be blank.")
    if isinstance(age, bool) or not isinstance(age, int) or age <= 0:
        raise ValueError("Age must be a positive integer.")
    if isinstance(marks, bool) or not isinstance(marks, (int, float)):
        raise ValueError("Marks must be numeric.")
    if not 0 <= marks <= 100:
        raise ValueError("Marks must be between 0 and 100.")

    return {"name": name, "age": age, "marks": marks}

try:
    student = make_student("Priya", 20, 88)
    print(student)
except ValueError as error:
    print("Invalid student record:", error)
```

This function validates already-converted values. Convert and validate raw user input separately.

## Exercise 35 — Customer Data Record

```python
customer = {
    "name": "Neha",
    "age": 25,
    "city": "Indore",
    "is_member": True,
    "purchases": [250.0, 500.0, 150.0],
}

total = sum(customer["purchases"])

print(f"Customer: {customer['name']}")
print(f"Age: {customer['age']}")
print(f"City: {customer['city']}")
print(f"Member: {customer['is_member']}")
print(f"Total purchases: {total:.2f}")
```

## General Testing Checklist

For a strong solution, also test:

- Empty strings and collections.
- Negative and zero values.
- Invalid numeric text.
- Missing dictionary keys.
- Duplicate values.
- Boundary marks such as 0 and 100.
- Lists containing one item or no items.
- Invalid discount percentages.

## Completion Criteria

You are ready for the next module when you can create variables, identify types, convert values safely, manipulate collections, explain references and mutability, and validate simple records.
