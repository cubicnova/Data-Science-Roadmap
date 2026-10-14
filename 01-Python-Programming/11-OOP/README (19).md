# 11 – Object-Oriented Programming (OOP) in Python

## Overview
Object-oriented programming organizes code around **objects** that combine data (attributes) and behavior (methods). Python supports classes, inheritance, encapsulation conventions, polymorphism, and special methods.

## Learning objectives
- Explain classes, objects, attributes, and methods.
- Define classes with `class` and initialize objects with `__init__`.
- Distinguish instance and class attributes.
- Use instance, class, and static methods.
- Apply inheritance, overriding, and polymorphism.
- Use properties to validate attribute updates.
- Implement special methods such as `__str__` and `__len__`.
- Understand composition and dataclasses.
- Design small, testable classes.

## 1. Classes and objects
A **class** defines a kind of object; an **object** is an instance of a class.

```python
class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def bark(self):
        return f"{self.name} says woof!"

pet = Dog("Milo", 3)
print(pet.name)
print(pet.bark())
```
`self` refers to the current instance. Python passes it automatically when an instance method is called.

## 2. `__init__` and instance attributes
`__init__` initializes a newly created instance. Attributes assigned through `self` belong to that object.
```python
class Student:
    def __init__(self, name, score):
        self.name = name
        self.score = score

first = Student("Asha", 92)
second = Student("Ravi", 84)
```

## 3. Instance attributes versus class attributes
A class attribute is shared through the class unless an instance shadows it. Use it for values that belong to the class as a whole.
```python
class Employee:
    company = "Example Ltd"
    def __init__(self, name):
        self.name = name
```
Avoid mutable class attributes for per-instance state:
```python
class Cart:
    def __init__(self):
        self.items = []  # a separate list for every instance
```

## 4. Instance, class, and static methods
- Instance method: receives `self`.
- Class method: receives `cls`.
- Static method: receives neither automatically.

```python
class Temperature:
    scale = "Celsius"
    def __init__(self, value):
        self.value = value
    def describe(self):
        return f"{self.value} degrees {self.scale}"
    @classmethod
    def from_fahrenheit(cls, value):
        return cls((value - 32) * 5 / 9)
    @staticmethod
    def is_above_freezing(value):
        return value > 0
```

## 5. Encapsulation and properties
A leading underscore (such as `_balance`) indicates internal use by convention; it is not strict privacy. Properties allow attribute-style access with validation.
```python
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
```

## 6. Inheritance and overriding
Inheritance lets a subclass reuse or specialize a base class. Use `super()` to call parent behavior.
```python
class Animal:
    def speak(self):
        return "Some sound"
class Cat(Animal):
    def speak(self):
        return "Meow"
class NamedDog(Animal):
    def __init__(self, name):
        super().__init__()
        self.name = name
```
Choose inheritance when the subtype relationship is meaningful; otherwise, composition may be simpler.

## 7. Polymorphism and duck typing
Different object types can respond to the same operation. Duck typing means an object is suitable if it supports the required behavior.
```python
def make_it_speak(animal):
    return animal.speak()

class Bird:
    def speak(self):
        return "Tweet"

print(make_it_speak(Bird()))
```

## 8. Special methods
Special methods let custom objects work with built-in operations.
```python
class Book:
    def __init__(self, title, pages):
        self.title = title
        self.pages = pages
    def __str__(self):
        return f"{self.title} ({self.pages} pages)"
    def __len__(self):
        return self.pages
```
Use `__str__` for user-friendly display and `__repr__` for a developer-oriented representation.

## 9. Composition
Composition builds a class from other objects and can be more flexible than deep inheritance.
```python
class Engine:
    def start(self):
        return "Engine started"
class Car:
    def __init__(self, engine):
        self.engine = engine
    def start(self):
        return self.engine.start()
```

## 10. Dataclasses
For classes mainly intended to store data, `dataclasses` can generate common methods.
```python
from dataclasses import dataclass, field

@dataclass
class DatasetInfo:
    name: str
    columns: list[str] = field(default_factory=list)
```
Use `field(default_factory=list)` for mutable defaults.

## 11. Validating state
```python
class BankAccount:
    def __init__(self, balance=0):
        if balance < 0:
            raise ValueError("balance cannot be negative")
        self._balance = balance
    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("amount must be positive")
        self._balance += amount
        return self._balance
```

## Common mistakes
- Forgetting `self` in instance methods.
- Confusing classes with instances.
- Using mutable class attributes for per-instance data.
- Forgetting `super().__init__()` when needed.
- Using inheritance when composition is a better fit.
- Assuming `_attribute` is truly private.
- Skipping validation in property setters.
- Putting unrelated responsibilities in one class.

## Best practices
- Give each class one clear responsibility.
- Use `PascalCase` class names and `snake_case` methods.
- Keep public interfaces predictable.
- Validate state during creation and updates.
- Prefer composition over unnecessary inheritance.
- Use dataclasses for simple data containers.
- Test behavior and edge cases.

## Quick reference
| Concept | Example |
|---|---|
| Class | `class Person:` |
| Constructor | `def __init__(self, name):` |
| Instance method | `def greet(self):` |
| Class method | `@classmethod` |
| Static method | `@staticmethod` |
| Property | `@property` |
| Inheritance | `class Child(Parent):` |
| Parent behavior | `super()` |
| Data class | `@dataclass` |
| String display | `def __str__(self):` |

## Module files
- `README.md` — theory, syntax, and examples
- `practice.md` — 35 exercises
- `examples.ipynb` — runnable notebook examples
- `solutions.md` — reference answers
