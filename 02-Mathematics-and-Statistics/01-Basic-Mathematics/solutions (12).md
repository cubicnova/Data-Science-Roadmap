# Basic Mathematics — Reference Solutions

```python
# 1. 18 + 6 * 3 = 36
# 2. (18 + 6) * 3 = 72
# 3. 3/5 = 0.6 = 60%
# 4. 450 * 22 / 100 = 99
# 5. (920 - 800) / 800 * 100 = 15%
# 6. (200 - 250) / 250 * 100 = -20% (a 20% decrease)
# 7. 4x + 7x - 3 = 11x - 3
# 8. 5*6 - 2 = 28
# 9. 4x + 9 = 37 -> x = 7
# 10. 7x - 5 = 30 -> x = 5
# 11. 2:5; if 2 parts = 18, one part = 9, so second = 45
# 12. 90/6 * 14 = 210
# 13. 2**6 = 64; 10**3 = 1000
# 14. 3**2 * 3**4 = 3**6 = 729
# 15. sqrt(196) = 14
# 16. log10(10000) = 4
# 17. log2(32) = 5
# 18. (17 - 5) / (6 - 2) = 3
# 19. f(3) = 4*3 - 2 = 10
# 20. (5 + 7 + 10 + 18) / 4 = 10
# 21. 1 + ... + 10 = 55
# 22. abs(-23) = 23; distance = abs(-4 - 9) = 13
# 23. 47 % 6 = 5
# 24. / returns true division; // returns floor division.
# 25.
part = 360 * 12 / 100

# 26.
def percentage_change(old_value, new_value):
    if old_value == 0:
        raise ValueError("Percentage change is undefined when old_value is zero.")
    return (new_value - old_value) / old_value * 100

# 27.
x = (29 - 8) / 3  # 7.0

# 28.
numbers = [3, 5, 7, 9, 11]
mean = sum(numbers) / len(numbers)

# 29.
prices = [100, 250, 80]
increased_prices = [price * 1.05 for price in prices]

# 30. 24:36 simplifies by dividing both values by 12 -> 2:3
# 31. There is no real number y such that 10**y = 0; positive bases raised
#     to finite real powers are positive, so log(0) is undefined.
# 32. slope = (y2-y1)/(x2-x1); it is undefined when x2 == x1 (vertical line).
# 33. (4 + 6 + 6 + 8 + 11) / 5 = 7

# 34.
def safe_percentage_change(old_value, new_value):
    if old_value == 0:
        return None  # caller can report that the percentage change is undefined
    return (new_value - old_value) / old_value * 100

# 35.
def calculate_two_numbers(a, b):
    result = {
        "sum": a + b,
        "product": a * b,
        "absolute_difference": abs(a - b),
    }
    result["quotient"] = a / b if b != 0 else None
    return result
```

## Challenge example

```python
data = [4, 6, 6, 8, 11]
mean = sum(data) / len(data)
data_range = max(data) - min(data)
percent_increase = (
    (max(data) - min(data)) / min(data) * 100
    if min(data) != 0 else None
)
absolute_errors = [abs(value - mean) for value in data]

print("Mean:", mean)
print("Range:", data_range)
print("Percentage increase:", percent_increase)
print("Absolute differences:", absolute_errors)
```

For this dataset, the mean is 7, range is 7, and the increase from 4 to 11 is 175%.
