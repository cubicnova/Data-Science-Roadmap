# Reference Solutions: Conditional Statements in Python

These are sample answers; alternative correct solutions are possible.

## Beginner: `if` and `if...else`

### 1. Hot temperature
```python
temperature = 34
if temperature > 30:
    print("Hot")
```

### 2. Positive number
```python
number = 8
if number > 0:
    print("Positive")
```

### 3. Even or odd
```python
number = 13
if number % 2 == 0:
    print("Even")
else:
    print("Odd")
```

### 4. Voting eligibility
```python
age = 19
if age >= 18:
    print("Eligible to vote")
else:
    print("Not eligible to vote")
```

### 5. Pass or fail
```python
score = 56
if score >= 40:
    print("Pass")
else:
    print("Fail")
```

### 6. Divisible by 5
```python
number = 45
if number % 5 == 0:
    print("Divisible by 5")
else:
    print("Not divisible by 5")
```

### 7. Missing username
```python
username = ""
if not username:
    print("Username missing")
```

### 8. Compare two numbers
```python
a, b = 10, 10
if a > b:
    print("First is larger")
elif b > a:
    print("Second is larger")
else:
    print("Both are equal")
```

### 9. Leap year
```python
year = 2024
if year % 400 == 0 or (year % 4 == 0 and year % 100 != 0):
    print("Leap year")
else:
    print("Not a leap year")
```

### 10. Zero or non-zero
```python
number = 0
if number == 0:
    print("Zero")
else:
    print("Non-zero")
```

## `if...elif...else`

### 11. Grade with validation
```python
score = 86
if not 0 <= score <= 100:
    print("Invalid score")
elif score >= 90:
    print("A")
elif score >= 80:
    print("B")
elif score >= 70:
    print("C")
elif score >= 60:
    print("D")
else:
    print("F")
```

### 12. Positive, negative, or zero
```python
number = -4
if number > 0:
    print("Positive")
elif number < 0:
    print("Negative")
else:
    print("Zero")
```

### 13. Age group
```python
age = 16
if age < 13:
    print("Child")
elif age <= 19:
    print("Teenager")
else:
    print("Adult")
```

### 14. Temperature category
```python
temperature = 22
if temperature < 15:
    print("Cold")
elif temperature <= 30:
    print("Moderate")
else:
    print("Hot")
```

### 15. Days in a month (non-leap year)
```python
month = 2
if month in (1, 3, 5, 7, 8, 10, 12):
    print(31)
elif month in (4, 6, 9, 11):
    print(30)
elif month == 2:
    print(28)
else:
    print("Invalid month")
```

### 16. Vowel or consonant
```python
character = "e"
if len(character) != 1 or not character.isalpha() or not character.isascii():
    print("Invalid input")
elif character.lower() in "aeiou":
    print("Vowel")
else:
    print("Consonant")
```

### 17. Triangle validation and type
```python
a, b, c = 3, 3, 4
if a <= 0 or b <= 0 or c <= 0 or a + b <= c or a + c <= b or b + c <= a:
    print("Invalid triangle")
elif a == b == c:
    print("Equilateral")
elif a == b or a == c or b == c:
    print("Isosceles")
else:
    print("Scalene")
```

### 18. Purchase discount
```python
amount = 1250
if amount >= 1000:
    final_amount = amount * 0.90
else:
    final_amount = amount
print(final_amount)  # 1125.0
```

### 19. Entry rules
```python
age = 21
has_id = True
if age >= 18 and has_id:
    print("Entry allowed")
else:
    print("Entry denied")
```

### 20. Day classification
```python
day = "Sunday"
if day in ("Saturday", "Sunday"):
    print("Weekend")
elif day in ("Monday", "Tuesday", "Wednesday", "Thursday", "Friday"):
    print("Weekday")
else:
    print("Invalid day")
```

## Intermediate: Nested and Combined Conditions

### 21. Basic login check
```python
expected_username = "student"
expected_password = "python123"
username = "student"
password = "python123"
if username == expected_username and password == expected_password:
    print("Login successful")
else:
    print("Invalid username or password")
```
Teaching example only: real applications must not store plaintext passwords.

### 22. Validate year and month
```python
year, month = 2026, 10
if year > 0:
    if 1 <= month <= 12:
        print("Valid year and month")
    else:
        print("Invalid month")
else:
    print("Year must be positive")
```

### 23. Largest of three without `max()`
```python
a, b, c = 12, 25, 18
if a >= b and a >= c:
    largest = a
elif b >= a and b >= c:
    largest = b
else:
    largest = c
print(largest)
```

### 24. Divisibility by 3 and 5
```python
number = 15
if number % 3 == 0 and number % 5 == 0:
    print("Divisible by both")
elif number % 3 == 0 or number % 5 == 0:
    print("Divisible by one")
else:
    print("Divisible by neither")
```

### 25. Access rules
```python
has_account = True
is_verified = True
is_suspended = False
if has_account and is_verified and not is_suspended:
    print("Access granted")
else:
    print("Access denied")
```

### 26. Safe first-item check
```python
items = ["admin", "reports"]
if items:
    if items[0] == "admin":
        print("Admin list")
    else:
        print("First item is not admin")
else:
    print("List is empty")
```

### 27. Score and attendance validation
```python
score = 72
attendance_percentage = 80
if not 0 <= score <= 100:
    print("Invalid score")
elif not 0 <= attendance_percentage <= 100:
    print("Invalid attendance")
elif score >= 40 and attendance_percentage >= 75:
    print("Eligible")
else:
    print("Not eligible")
```

### 28. Conditional expression
```python
age = 17
status = "Adult" if age >= 18 else "Minor"
print(status)
```

### 29. Count positive, negative, and zero values
```python
values = [4, -2, 0, 8, -1, 0]
positive = negative = zero = 0
for value in values:
    if value > 0:
        positive += 1
    elif value < 0:
        negative += 1
    else:
        zero += 1
print("Positive:", positive)
print("Negative:", negative)
print("Zero:", zero)
```

### 30. Placeholder with `pass`
```python
score = 85
if score >= 50:
    pass
else:
    print("Try again")
```
When the condition is true, `pass` does nothing; execution continues after the conditional.

## Applied Data Science

### 31. Score classification
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

### 32. Count temperatures above 30
```python
temperatures = [28, 32, 35, 29, 31]
count = 0
for temperature in temperatures:
    if temperature > 30:
        count += 1
print(count)  # 3
```

### 33. Transaction labels
```python
amounts = [500, 12000, 7500, 25000]
for amount in amounts:
    label = "Large" if amount >= 10000 else "Regular"
    print(amount, label)
```

### 34. Basic data-quality rule
```python
record = {"age": 150, "email": ""}
if record["age"] < 0 or record["age"] > 120 or not record["email"].strip():
    print("Flag record for review")
else:
    print("Record passes this basic check")
```
Production validation should also handle missing keys, non-numeric ages, and malformed email addresses.

### 35. Menu-driven calculator
```python
a, b = 20, 5
operation = "divide"  # Try add, subtract, multiply, divide
if operation == "add":
    result = a + b
elif operation == "subtract":
    result = a - b
elif operation == "multiply":
    result = a * b
elif operation == "divide":
    if b == 0:
        result = "Cannot divide by zero"
    else:
        result = a / b
else:
    result = "Unknown operation"
print(result)
```
