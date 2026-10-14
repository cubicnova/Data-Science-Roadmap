# Python Comprehensions — Reference Solutions

These are example solutions. Variable names and formatting may differ from your own correct answers.

```python
# 1
squares = [n ** 2 for n in range(1, 11)]

# 2
cubes = [n ** 3 for n in range(1, 9)]

# 3
evens = [n for n in range(31) if n % 2 == 0]

# 4
odds = [n for n in range(1, 26) if n % 2 == 1]

# 5
words = ["comprehension", "python", "data"]
lengths = [len(word) for word in words]

# 6
uppercase_words = [word.upper() for word in words]

# 7
cleaned = [value.strip() for value in [" Ada ", " Grace ", "Linus "]]

# 8
positive = [n for n in [-4, 0, 7, -1, 3] if n > 0]

# 9
long_words = [word for word in words if len(word) > 5]

# 10
multiples = [n for n in range(1, 101) if n % 3 == 0 and n % 5 == 0]

# 11
labels = ["even" if n % 2 == 0 else "odd" for n in range(1, 11)]

# 12
celsius = [0, 20, 30, 100]
fahrenheit = [(c * 9 / 5) + 32 for c in celsius]

# 13
first_letters = [word[0] for word in ["apple", "", "banana", "pear"] if word]

# 14
number_squares = [(n, n ** 2) for n in range(1, 6)]

# 15
nonempty = [s for s in ["hello", "", " ", "world"] if s]

# 16
first_letters = {word[0] for word in ["apple", "apricot", "banana"]}

# 17
unique_lengths = {len(word) for word in ["cat", "dog", "python", "owl"]}

# 18
square_map = {n: n ** 2 for n in range(1, 8)}

# 19
word_lengths = {word: len(word) for word in ["red", "green", "blue"]}

# 20
prices = {"pen": 10, "book": 80}
prices_with_tax = {item: price * 1.10 for item, price in prices.items()}

# 21
scores = {"Asha": 78, "Ben": 42, "Chen": 91}
high_scores = {name: score for name, score in scores.items() if score > 50}

# 22
names = ["ada", "grace"]
uppercase_names = {name: name.upper() for name in names}

# 23
nested = [[1, 2], [3, 4], [5]]
flat = [value for row in nested for value in row]

# 24
pairs = [(x, y) for x in range(3) for y in range(3)]

# 25
matrix = [[1, 7, 3], [8, 2, 9]]
large_values = [value for row in matrix for value in row if value > 5]

# 26
matrix = [[1, 2, 3], [4, 5, 6]]
transposed = [[row[col] for row in matrix] for col in range(len(matrix[0]))]

# 27
table = [[row * col for col in range(1, 6)] for row in range(1, 6)]

# 28
sentence = "Comprehensions are useful"
vowels = [char.lower() for char in sentence if char.lower() in "aeiou"]

# 29
values = [-5, 0, 3, -2, 8]
nonnegative = [value if value >= 0 else 0 for value in values]

# 30
words = ["Apple", "pear", "plum", "Avocado"]
with_a = [word for word in words if "a" in word.lower()]

# 31
parity = {n: ("even" if n % 2 == 0 else "odd") for n in range(1, 11)}

# 32
total = sum(n ** 2 for n in range(1, 101))

# 33
# A generator is useful when values can be consumed one at a time without storing
# the entire result. A list is useful when you need to reuse or index the values.
values_list = [n ** 2 for n in range(1, 6)]
values_generator = (n ** 2 for n in range(1, 6))

# 34
records = [{"name": "Asha", "score": 81}, {"name": "Ben", "score": 65}]
passing_scores = [record["score"] for record in records if record["score"] >= 70]

# 35
names = ["Asha", "Ben", "Asha", "Chen"]
name_lengths = {name: len(name) for name in names}
```

## Notes

- Exercise 15 checks truthiness, so `" "` (a string containing spaces) is retained. To remove whitespace-only strings, use `if s.strip()`.
- Exercise 35 naturally creates one dictionary entry per distinct key; repeated names overwrite the same key with the same length.
- In exercise 26, the example assumes a non-empty rectangular matrix.
