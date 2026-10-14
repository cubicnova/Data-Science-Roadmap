# Reference Solutions — 11 OOP

These are sample answers; other correct designs are possible.

```python
# 1
class Person:
    def __init__(self, name):
        self.name = name

# 2
class Dog:
    def __init__(self, name):
        self.name = name
    def bark(self):
        return f"{self.name} says woof!"

# 3: self refers to the current instance passed to an instance method.

# 4
class Student:
    def __init__(self, name, score):
        self.name = name
        self.score = score
s1 = Student("Asha", 92)
s2 = Student("Ravi", 84)

# 5
class StudentWithScore:
    def __init__(self, score):
        self.score = score
    def get_score(self):
        return self.score

# 6: A class defines a type; an object is an instance of it.

# 7
class Employee:
    company = "Example Ltd"
    def __init__(self, name):
        self.name = name

# 8: Avoid this shared mutable class attribute pattern.
class BadCart:
    items = []
class Cart:
    def __init__(self):
        self.items = []

# 9
class User:
    def __init__(self, name):
        self.name = name
    @classmethod
    def from_text(cls, text):
        return cls(text.strip())

# 10
class NumberTools:
    @staticmethod
    def is_even(number):
        return number % 2 == 0

# 11
class BankAccount:
    def __init__(self, balance=0):
        if balance < 0:
            raise ValueError("balance cannot be negative")
        self._balance = balance

# 12
class AccountWithDeposit(BankAccount):
    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("amount must be positive")
        self._balance += amount
        return self._balance

# 13
class ReadOnlyAccount:
    def __init__(self, balance):
        self._balance = balance
    @property
    def balance(self):
        return self._balance

# 14
class Product:
    def __init__(self, price):
        self.price = price
    @property
    def price(self):
        return self._price
    @price.setter
    def price(self, value):
        if value < 0:
            raise ValueError("price cannot be negative")
        self._price = value

# 15
class Animal:
    def speak(self):
        return "Some sound"
class Cat(Animal):
    def speak(self):
        return "Meow"

# 16
class NamedAnimal:
    def __init__(self, name):
        self.name = name
class NamedDog(NamedAnimal):
    def __init__(self, name, trained=False):
        super().__init__(name)
        self.trained = trained

# 17
class Bird:
    def speak(self):
        return "Tweet"
def speak_twice(animal):
    return animal.speak(), animal.speak()

# 18
def make_it_speak(obj):
    return obj.speak()

# 19
class Book:
    def __init__(self, title, pages):
        self.title = title
        self.pages = pages
    def __str__(self):
        return f"{self.title} ({self.pages} pages)"

# 20
class Playlist:
    def __init__(self, songs):
        self.songs = list(songs)
    def __len__(self):
        return len(self.songs)

# 21: __str__ is readable display; __repr__ is developer-oriented.

# 22
class Engine:
    def start(self):
        return "Engine started"
class Car:
    def __init__(self, engine):
        self.engine = engine
    def start(self):
        return self.engine.start()

# 23
from dataclasses import dataclass
@dataclass
class Point:
    x: float
    y: float

# 24
from dataclasses import dataclass, field
@dataclass
class Team:
    name: str
    members: list[str] = field(default_factory=list)

# 25
class NamedThing:
    def __init__(self, name):
        if not name.strip():
            raise ValueError("name cannot be empty")
        self.name = name

# 26 and 27
class Rectangle:
    def __init__(self, width, height):
        if width < 0 or height < 0:
            raise ValueError("dimensions must be non-negative")
        self.width = width
        self.height = height
    def area(self):
        return self.width * self.height
    def perimeter(self):
        return 2 * (self.width + self.height)

# 28
class InstanceCounter:
    count = 0
    def __init__(self):
        type(self).count += 1

# 29: A leading underscore is a convention, not strict privacy.

# 30
class Base:
    def describe(self):
        return "base"
class Child(Base):
    def describe(self):
        return super().describe() + " child"

# 31
class ShoppingCart:
    def __init__(self):
        self.items = []

# 32
class PricedCart:
    def __init__(self, items):
        self.items = list(items)
    def total(self):
        return sum(price for name, price in self.items)

# 33
class PersonRecord:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    @classmethod
    def from_dict(cls, data):
        return cls(name=data["name"], age=data["age"])

# 34: Composition can reduce inheritance coupling and make components easier to swap.
# 35: Test valid construction, invalid attributes, and boundary values.
```

## Notes
- Initialize mutable instance state in `__init__`.
- Prefer composition when a class “has a” component rather than “is a” specialized type.
- Public methods form the interface callers should rely on.
