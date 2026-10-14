# Reference Solutions: Data Structures in Python

These are sample solutions. Other correct implementations are possible.

## Beginner: Lists and Tuples

### 1. First, middle, and last
```python
numbers = [10, 20, 30, 40, 50]
print(numbers[0], numbers[len(numbers) // 2], numbers[-1])
```

### 2. Append an item
```python
numbers = [1, 2, 3]
numbers.append(4)
print(numbers)
```

### 3. Insert at index 2
```python
numbers = [10, 20, 40, 50]
numbers.insert(2, 30)
print(numbers)  # [10, 20, 30, 40, 50]
```

### 4. Remove a value safely
```python
numbers = [1, 2, 3, 2]
value = 8
if value in numbers:
    numbers.remove(value)
else:
    print("Value not found")
```

### 5. Slicing
```python
items = [10, 20, 30, 40, 50, 60]
print(items[:3])
print(items[-2:])
```

### 6. Sort ascending and descending
```python
numbers = [5, 1, 8, 2]
print(sorted(numbers))
print(sorted(numbers, reverse=True))
```

### 7. Count occurrences
```python
numbers = [2, 3, 2, 4, 2]
print(numbers.count(2))  # 3
```

### 8. Find an index safely
```python
items = ["red", "blue", "green"]
target = "blue"
if target in items:
    print(items.index(target))
else:
    print("Not found")
```

### 9. List to tuple
```python
values = [1, 2, 3, 4, 5]
result = tuple(values)
print(result, len(result))
```

### 10. One-item tuple
```python
single = (5,)
print(type(single))  # <class 'tuple'>
```
The comma makes it a tuple; `(5)` by itself is an integer.

## Sets and Dictionaries

### 11. Remove duplicates
```python
values = [1, 2, 2, 3, 3, 3]
print(set(values))
```

### 12. Set operations
```python
a = {1, 2, 3}
b = {3, 4, 5}
print(a | b)  # union
print(a & b)  # intersection
print(a - b)  # difference
```

### 13. Membership in a set
```python
allowed = {"read", "write", "execute"}
print("write" in allowed)
```

### 14. Create an empty set
```python
values = set()
values.update([1, 2, 3])
print(values)
```

### 15. Student dictionary
```python
student = {"name": "Asha", "roll_number": 12, "marks": 88}
print(student)
```

### 16. Safe dictionary lookup
```python
student = {"name": "Asha", "marks": 88}
print(student.get("email", "Not provided"))
```

### 17. Add and update dictionary entries
```python
student = {"name": "Asha", "marks": 88}
student["city"] = "Indore"
student["marks"] = 92
print(student)
```

### 18. Iterate over key-value pairs
```python
student = {"name": "Asha", "marks": 92}
for key, value in student.items():
    print(key, value)
```

### 19. Remove a key safely
```python
student = {"name": "Asha", "marks": 92}
removed = student.pop("email", None)
print(removed)  # None if absent
```

### 20. Word frequencies
```python
words = ["data", "python", "data", "science"]
frequencies = {}
for word in words:
    frequencies[word] = frequencies.get(word, 0) + 1
print(frequencies)
```

## Strings, Mutability, and Nested Structures

### 21. String processing
```python
sentence = "Python makes data work"
lowercase = sentence.lower()
words = lowercase.split()
print(lowercase)
print(words)
print(len(words))
```

### 22. Reverse a string
```python
text = "Python"
print(text[::-1])
```
Strings are immutable; operations create new strings rather than modifying the original object in place.

### 23. References versus copies
```python
original = [1, 2]
alias = original
alias.append(3)
print(original)  # [1, 2, 3]

copy = original.copy()
copy.append(4)
print(original)  # [1, 2, 3]
print(copy)      # [1, 2, 3, 4]
```

### 24. Student dictionaries
```python
students = [
    {"name": "Asha", "marks": 90},
    {"name": "Ravi", "marks": 84},
]
for student in students:
    print(student["name"], student["marks"])
```

### 25. Nested lists
```python
matrix = [[1, 2, 3], [4, 5, 6]]
print(matrix[1][2])  # 6
```

### 26. Products over 500
```python
products = {"keyboard": 800, "mouse": 350, "monitor": 7000}
for product, price in products.items():
    if price > 500:
        print(product, price)
```

### 27. Preserve order while removing duplicates
```python
values = [3, 1, 3, 2, 1]
unique = list(dict.fromkeys(values))
print(unique)  # [3, 1, 2]
```

### 28. Common values
```python
first = [1, 2, 3, 4]
second = [3, 4, 5, 6]
print(set(first) & set(second))  # {3, 4}
```

### 29. Group names by first letter
```python
names = ["Asha", "Anil", "Ravi", "Mina"]
grouped = {}
for name in names:
    initial = name[0].upper()
    grouped.setdefault(initial, []).append(name)
print(grouped)
```

### 30. Filter a dictionary
```python
marks = {"Asha": 88, "Ravi": 42, "Mina": 75}
passed = {name: mark for name, mark in marks.items() if mark >= 50}
print(passed)
```

## Applied Data Science

### 31. Min, max, and average without `min()` or `max()`
```python
scores = [82, 47, 35, 91, 68]
if not scores:
    print("No scores available")
else:
    smallest = largest = total = scores[0]
    total = 0
    for score in scores:
        if score < smallest:
            smallest = score
        if score > largest:
            largest = score
        total += score
    average = total / len(scores)
    print(smallest, largest, average)
```

### 32. Total transaction amounts by category
```python
transactions = [
    {"category": "food", "amount": 250},
    {"category": "travel", "amount": 500},
    {"category": "food", "amount": 150},
]
totals = {}
for transaction in transactions:
    category = transaction["category"]
    totals[category] = totals.get(category, 0) + transaction["amount"]
print(totals)  # {'food': 400, 'travel': 500}
```

### 33. Compare customer IDs
```python
customers_a = {101, 102, 103, 104}
customers_b = {103, 104, 105}
print("Both:", customers_a & customers_b)
print("Only A:", customers_a - customers_b)
print("Only B:", customers_b - customers_a)
```

### 34. Name-to-score dictionary
```python
records = [
    {"name": "Asha", "score": 91},
    {"name": "Ravi", "score": 85},
]
score_by_name = {record["name"]: record["score"] for record in records}
print(score_by_name)
```
If names repeat, the later score replaces the earlier one. Choose a different structure if duplicates must be preserved.

### 35. Average marks per student
```python
students = [
    {"name": "Asha", "marks": [88, 91, 85]},
    {"name": "Ravi", "marks": [78, 82, 80]},
]
summary = []
for student in students:
    marks = student["marks"]
    average = sum(marks) / len(marks) if marks else None
    summary.append({"name": student["name"], "average": average})
print(summary)
```
