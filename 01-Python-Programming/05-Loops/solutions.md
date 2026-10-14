# Reference Solutions: Loops in Python

These are sample solutions. Other correct approaches are possible.

## Beginner: `for` Loops and `range()`

### 1. Print 1 through 10
```python
for number in range(1, 11):
    print(number)
```

### 2. Even numbers 2 through 20
```python
for number in range(2, 21, 2):
    print(number)
```

### 3. Characters in a string
```python
for character in "Python":
    print(character)
```

### 4. Iterate over languages
```python
languages = ["Python", "Java", "C++", "R", "SQL"]
for language in languages:
    print(language)
```

### 5. Squares from 1 through 10
```python
for number in range(1, 11):
    print(number, number ** 2)
```

### 6. Sum from 1 through 100
```python
total = 0
for number in range(1, 101):
    total += number
print(total)  # 5050
```

### 7. Multiplication table
```python
number = 7
for multiplier in range(1, 11):
    print(f"{number} x {multiplier} = {number * multiplier}")
```

### 8. Count vowels
```python
text = "Data Science"
vowel_count = 0
for character in text.lower():
    if character in "aeiou":
        vowel_count += 1
print(vowel_count)
```

### 9. Count even numbers
```python
numbers = [2, 7, 8, 11, 14, 15]
count = 0
for number in numbers:
    if number % 2 == 0:
        count += 1
print(count)  # 3
```

### 10. Iterate over a dictionary
```python
marks = {"Python": 88, "Statistics": 92, "SQL": 85}
for subject, mark in marks.items():
    print(subject, mark)
```

## `while` Loops and Control Statements

### 11. Print 1 through 10 with `while`
```python
number = 1
while number <= 10:
    print(number)
    number += 1
```

### 12. Countdown
```python
number = 10
while number >= 1:
    print(number)
    number -= 1
print("Go!")
```

### 13. Add until total reaches 100
```python
total = 0
number = 1
while total < 100:
    total += number
    number += 1
print("Total:", total)
print("Last number added:", number - 1)
```

### 14. Stop at 7 with `break`
```python
for number in range(1, 11):
    if number == 7:
        break
    print(number)
```

### 15. Skip 5 with `continue`
```python
for number in range(1, 11):
    if number == 5:
        continue
    print(number)
```

### 16. Simulated secret-word attempts
```python
guesses = ["hello", "python", "loops"]
secret = "loops"
for guess in guesses:
    if guess == secret:
        print("Correct")
        break
else:
    print("No correct guess")
```

### 17. Factorial
```python
number = 5
factorial = 1
for value in range(1, number + 1):
    factorial *= value
print(factorial)  # 120
```
For this approach, `number` should be a non-negative integer. `0!` is 1.

### 18. Reverse a string without slicing
```python
text = "Python"
reversed_text = ""
for character in text:
    reversed_text = character + reversed_text
print(reversed_text)  # nohtyP
```

### 19. Largest value without `max()`
```python
numbers = [8, 3, 12, 5]
if not numbers:
    print("List is empty")
else:
    largest = numbers[0]
    for number in numbers[1:]:
        if number > largest:
            largest = number
    print(largest)  # 12
```

### 20. Use `pass`
```python
for number in range(3):
    pass  # Placeholder; no action is performed
print("Loop finished")
```
`pass` does nothing and does not stop the loop.

## Nested Loops and Iteration Tools

### 21. 5-by-5 square of stars
```python
for row in range(5):
    for column in range(5):
        print("*", end="")
    print()
```

### 22. Right triangle of stars
```python
for row in range(1, 6):
    for column in range(row):
        print("*", end="")
    print()
```

### 23. Print pairs
```python
for a in range(1, 4):
    for b in range(1, 3):
        print((a, b))
```

### 24. Multiplication tables 1 through 5
```python
for number in range(1, 6):
    for multiplier in range(1, 11):
        print(f"{number} x {multiplier} = {number * multiplier}")
    print()
```

### 25. Use `enumerate()`
```python
languages = ["Python", "Java", "SQL"]
for position, language in enumerate(languages, start=1):
    print(position, language)
```

### 26. Use `zip()`
```python
names = ["Asha", "Ravi", "Mina"]
scores = [91, 85, 94]
for name, score in zip(names, scores):
    print(name, score)
```

### 27. Search with `break` and loop `else`
```python
values = [4, 8, 12, 16]
target = 12
for value in values:
    if value == target:
        print("Found")
        break
else:
    print("Not found")
```

### 28. Filter negative values
```python
values = [4, -2, 0, -8, 9]
non_negative = []
for value in values:
    if value >= 0:
        non_negative.append(value)
print(non_negative)  # [4, 0, 9]
```

### 29. Equal corresponding positions
```python
first = [1, 2, 3, 4]
second = [1, 9, 3]
equal_count = 0
for a, b in zip(first, second):
    if a == b:
        equal_count += 1
print(equal_count)  # 2
```
`zip()` compares corresponding positions only up to the shorter list.

### 30. Number pattern
```python
for row in range(1, 6):
    for number in range(1, row + 1):
        print(number, end=" ")
    print()
```

## Applied Data Science

### 31. Filter passing scores
```python
scores = [82, 47, 35, 91, 68]
passing = []
for score in scores:
    if score >= 40:
        passing.append(score)
print(passing)  # [82, 47, 91, 68]
```

### 32. Mean with empty-list handling
```python
def calculate_mean(numbers):
    if not numbers:
        return None
    total = 0
    for number in numbers:
        total += number
    return total / len(numbers)

print(calculate_mean([2, 4, 6]))  # 4.0
print(calculate_mean([]))         # None
```

### 33. Count and total large transactions
```python
transactions = [500, 12000, 7500, 25000, 10000]
large_count = 0
large_total = 0
for amount in transactions:
    if amount >= 10000:
        large_count += 1
        large_total += amount
print("Count:", large_count)  # 3
print("Total:", large_total)  # 47000
```

### 34. Classify temperatures and count categories
```python
temperatures = [8, 18, 31, 25, 12, 35]
counts = {"Cold": 0, "Moderate": 0, "Hot": 0}
for temperature in temperatures:
    if temperature < 15:
        category = "Cold"
    elif temperature <= 30:
        category = "Moderate"
    else:
        category = "Hot"
    counts[category] += 1
    print(temperature, category)
print(counts)
```

### 35. Frequency dictionary
```python
values = [2, 3, 2, 5, 3, 2]
frequencies = {}
for value in values:
    if value in frequencies:
        frequencies[value] += 1
    else:
        frequencies[value] = 1
print(frequencies)  # {2: 3, 3: 2, 5: 1}
```
