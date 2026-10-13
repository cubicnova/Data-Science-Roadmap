# Reference Solutions: Operators in Python

These are sample solutions. Other correct implementations are possible. Run the code and make sure you understand each expression rather than memorizing the output.

## Beginner: Arithmetic and Assignment

### 1. Basic arithmetic
```python
a, b = 17, 5
print(a + b)  # 22
print(a - b)  # 12
print(a * b)  # 85
print(a / b)  # 3.4
```

### 2. Remainder
```python
print(47 % 6)  # 5
```

### 3. True division and floor division
```python
print(29 / 4)   # 7.25
print(29 // 4)  # 7
```

### 4. Exponentiation
```python
print(3 ** 5)  # 243
```

### 5. Product total
```python
price = 275
quantity = 4
print(price * quantity)  # 1100
```

### 6. Seconds conversion
```python
total_seconds = 367
minutes = total_seconds // 60
seconds = total_seconds % 60
print(minutes, "minutes and", seconds, "seconds")
# 6 minutes and 7 seconds
```

### 7. Augmented assignment
```python
value = 12
value += 3
print(value)  # 15
value -= 5
print(value)  # 10
value *= 2
print(value)  # 20
value /= 4
print(value)  # 5.0
```

### 8. Rectangle area
```python
length = 12.5
width = 4
area = length * width
print(area)  # 50.0
```

### 9. Average
```python
numbers = [12, 18, 25, 45]
average = sum(numbers) / len(numbers)
print(average)  # 25.0
```

### 10. Even or odd
```python
number = 17
if number % 2 == 0:
    print("Even")
else:
    print("Odd")
```

## Beginner: Comparisons and Logic

### 11. Compare two numbers
```python
a, b = 8, 11
if a > b:
    print("First is greater")
elif a < b:
    print("First is less")
else:
    print("Equal")
```

### 12. Passing score
```python
score = 76
print(score >= 40)  # True
```

### 13. Inclusive temperature range
```python
temperature = 25
print(18 <= temperature <= 30)  # True
```

### 14. Entry requirements
```python
age = 20
has_id = True
print(age >= 18 and has_id)  # True
```

### 15. Empty username
```python
username = ""
print(not username)  # True
```

### 16. Between 10 and 20
```python
number = 15
print(number > 10 and number < 20)  # True
# Equivalent: print(10 < number < 20)
```

### 17. Weekend check
```python
day = "Saturday"
print(day == "Saturday" or day == "Sunday")  # True
# Another option: day in ("Saturday", "Sunday")
```

### 18. Outside a range
```python
value = 120
outside = value < 1 or value > 100
print(outside)  # True
```

### 19. Short-circuit evaluation
```python
# With `and`, if the left side is false, the whole condition is false,
# so Python does not need to evaluate the right side.
is_ready = False
if is_ready and 10 / 0 > 1:
    print("Ready")
# No ZeroDivisionError occurs because the right side is skipped.
```

### 20. Safe first-item check
```python
items = []
if items and items[0] == "admin":
    print("Admin")
else:
    print("Not an admin-first list")
```

## Intermediate: Membership, Identity, and Precedence

### 21. Membership in a list
```python
tools = ["Python", "Pandas", "NumPy"]
print("Pandas" in tools)  # True
```

### 22. Dictionary membership
```python
record = {"name": "Asha", "score": 90}
print("score" in record)  # True: checks keys
print(90 in record)      # False: values are not checked by default
print(90 in record.values())  # True
```

### 23. Equality versus identity
```python
first = [1, 2, 3]
second = [1, 2, 3]
print(first == second)  # True: equal contents
print(first is second)  # False: different objects
```

### 24. Check for None
```python
result = None
print(result is None)  # True
```

### 25. Precedence
```python
print(2 + 3 * 4)  # 14: multiplication first
```

### 26. Parentheses
```python
print((2 + 3) * 4)  # 20
```

### 27. Exponentiation associativity
```python
print(2 ** 3 ** 2)  # 512
# Exponentiation groups from the right: 2 ** (3 ** 2) = 2 ** 9.
```

### 28. Clear Boolean condition
```python
age = 24
has_ticket = True
is_vip = False
allowed = (age >= 18) and (has_ticket or is_vip)
print(allowed)  # True
```

## Intermediate: Bitwise Operators

### 29. Bitwise AND, OR, and XOR
```python
print(bin(6))   # 0b110
print(bin(3))   # 0b11
print(6 & 3)    # 2
print(6 | 3)    # 7
print(6 ^ 3)    # 5
```

### 30. Bit shifting
```python
print(3 << 2)   # 12
print(16 >> 2)  # 4
```

### 31. Permission flags
```python
READ = 1
WRITE = 2
EXECUTE = 4

permissions = READ | WRITE
print(permissions)  # 3
print((permissions & WRITE) != 0)  # True
print((permissions & EXECUTE) != 0)  # False
```

### 32. Bitwise NOT
```python
print(~6)  # -7
# For Python integers, ~x is equivalent to -x - 1.
```

## Applied Data Science

### 33. Passing scores
```python
scores = [82, 47, 35, 91, 68]
passing = [score for score in scores if score >= 40]
print(passing)  # [82, 47, 91, 68]
```

### 34. Filter a price range
```python
prices = [100, 250, 80, 500]
selected = [price for price in prices if 100 < price <= 300]
print(selected)  # [250]
```

### 35. Percentage of even values
```python
def even_percentage(numbers):
    if not numbers:
        return 0.0  # Defined convention for an empty list
    even_count = sum(number % 2 == 0 for number in numbers)
    return even_count / len(numbers) * 100

print(even_percentage([1, 2, 3, 4]))  # 50.0
print(even_percentage([]))            # 0.0
```

For an empty list, another reasonable design is to return `None` or raise a clear exception. The important point is to decide explicitly rather than divide by zero.
