# Practice Exercises — 11 OOP

Complete all 35 exercises. Focus on behavior and object state, and test invalid inputs as well as normal use cases.

## 1. Create a `Person` class with a name attribute.

**Hint:** Initialize `self.name` in `__init__`.

## 2. Create a `Dog` class with a `bark()` method.

**Hint:** Return a short message from the method.

## 3. Explain the role of `self` in an instance method.

**Hint:** It refers to the current instance.

## 4. Create two `Student` objects with different names and scores.

**Hint:** Instantiate the same class twice.

## 5. Add an instance method that returns a student's score.

**Hint:** Read and return `self.score`.

## 6. Explain the difference between a class and an object.

**Hint:** A class defines a type; an object is an instance.

## 7. Create an `Employee` class with a class attribute `company`.

**Hint:** Declare it in the class body.

## 8. Show why a mutable class attribute can cause shared state.

**Hint:** Mutate a list defined at class level across two instances.

## 9. Create a class with a `@classmethod` constructor from a string.

**Hint:** Use `cls(...)` in the alternate constructor.

## 10. Create a `@staticmethod` that checks whether a number is even.

**Hint:** It needs no `self` or `cls`.

## 11. Create a `BankAccount` class with a validated non-negative starting balance.

**Hint:** Raise `ValueError` for invalid balance.

## 12. Add a deposit method that rejects zero and negative amounts.

**Hint:** Validate before updating balance.

## 13. Add a read-only `balance` property.

**Hint:** Use `@property` without a setter.

## 14. Create a property setter that rejects a negative product price.

**Hint:** Validate inside the setter.

## 15. Create a base `Animal` class and a `Cat` subclass that overrides `speak()`.

**Hint:** Redefine `speak()` in the subclass.

## 16. Use `super()` in a subclass initializer.

**Hint:** Call the parent initializer with required arguments.

## 17. Explain polymorphism with two classes that implement `speak()`.

**Hint:** Call the same method on different objects.

## 18. Write a function that accepts any object with a `speak()` method.

**Hint:** Use duck typing and call the method.

## 19. Implement `__str__` for a `Book` class.

**Hint:** Return a readable string.

## 20. Implement `__len__` for a `Playlist` class based on its songs.

**Hint:** Return the length of the songs collection.

## 21. Explain the difference between `__str__` and `__repr__`.

**Hint:** One is user-friendly; the other is developer-oriented.

## 22. Create an `Engine` class and compose it inside a `Car` class.

**Hint:** Pass an Engine instance into Car.

## 23. Use `@dataclass` to define a `Point` with x and y.

**Hint:** Import `dataclass` and annotate fields.

## 24. Use `field(default_factory=list)` for a dataclass list field.

**Hint:** Avoid sharing a mutable default.

## 25. Create a class that raises `ValueError` when a required name is empty.

**Hint:** Validate input in `__init__`.

## 26. Create a `Rectangle` class with an `area()` method.

**Hint:** Return width times height.

## 27. Add a `perimeter()` method to the rectangle.

**Hint:** Return twice the sum of width and height.

## 28. Create a class attribute that counts created instances.

**Hint:** Increment a class variable in `__init__`.

## 29. Explain why `_name` is not truly private in Python.

**Hint:** It is a convention, not enforced access control.

## 30. Override a parent method and call the parent's implementation using `super()`.

**Hint:** Call `super().method_name()`.

## 31. Create a `ShoppingCart` class that stores items per instance.

**Hint:** Initialize a fresh list in `__init__`.

## 32. Add a method that totals cart items represented as `(name, price)` tuples.

**Hint:** Sum the price values.

## 33. Write a class method that creates an object from a dictionary.

**Hint:** Use `@classmethod` and `cls(...)`.

## 34. Give two reasons to prefer composition over inheritance.

**Hint:** It can reduce coupling and allow flexible component replacement.

## 35. List three tests for a class that validates its attributes.

**Hint:** Test valid values, invalid values, and boundary cases.

