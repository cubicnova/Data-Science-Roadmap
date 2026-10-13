# Python Variables and Data Types — Practice Workbook

This workbook contains **35 exercises**, progressing from beginner concepts to intermediate programming tasks.

## Learning Objectives

- Create and use variables with clear names.
- Identify and convert Python data types.
- Manipulate strings and collections.
- Explain mutability and references.
- Validate input and handle common errors.
- Apply variables and data types to practical problems.

## Instructions

1. Attempt each problem before consulting `solutions.md`.
2. Use descriptive names and readable formatting.
3. Test normal, boundary, and invalid inputs where relevant.
4. Record assumptions if the problem does not specify every detail.

---

## Section 1 — Variables and Assignment

### Exercise 1: Personal Profile
Create variables for your name, age, city, qualification, and career goal. Print them in a readable format.

### Exercise 2: Multiple Assignment
Assign `10`, `20`, and `30` to three variables in one statement. Print all three values.

### Exercise 3: Variable Swapping
Swap the values of two variables without creating a third variable.

### Exercise 4: Identifier Rules
For each identifier, decide whether it is valid and explain why:

```python
student_name
2students
total_marks
first-name
class
_private
value2
```

### Exercise 5: Dynamic Typing
Assign an integer to a variable and print its type. Then assign a string to the same name and print its type again.

---

## Section 2 — Built-in Data Types

### Exercise 6: Type Identification
Create one example each of `int`, `float`, `complex`, `str`, `bool`, `list`, `tuple`, `set`, `dict`, and `NoneType`. Print each value and its type.

### Exercise 7: Numeric Operations
Store two numbers and calculate their sum, difference, product, and quotient. Prevent division by zero.

### Exercise 8: String Formatting
Store a person's name, age, and city. Display them in one sentence using an f-string.

### Exercise 9: Collection Creation
Create a list of five subjects, a tuple of three coordinates, a set of five numbers, and a dictionary containing student information. Print each collection.

### Exercise 10: Unique Values
Given the list below, create a list of unique values while preserving the order of first occurrence.

```python
values = [10, 20, 10, 30, 20, 40, 30]
```

---

## Section 3 — Type Conversion

### Exercise 11: String to Integer
Convert `"250"` into an integer and add `50`.

### Exercise 12: Integer to Float
Convert `100` to a floating-point value and display the value and its type.

### Exercise 13: Numeric Input
Accept two numbers from the user and display their sum. Handle invalid input.

### Exercise 14: Conversion Errors
Attempt to convert `"Python"` to an integer. Explain the exception and write a safer version that handles invalid input.

### Exercise 15: Boolean Conversion
Test `bool()` with `0`, `1`, `""`, `"Python"`, `[]`, `[0]`, and `None`. Record and explain the results.

---

## Section 4 — Strings and Formatting

### Exercise 16: String Length
Store a sentence and display its character count using `len()`.

### Exercise 17: String Manipulation
Convert a string to uppercase, lowercase, and title case.

### Exercise 18: Full Name
Accept a first name and last name, remove surrounding whitespace, and combine them into a full name.

### Exercise 19: Formatted Receipt
Store a product name, quantity, and unit price. Calculate the total and print a receipt with prices formatted to two decimal places.

### Exercise 20: Data Cleaning
Given a string with extra leading and trailing spaces, remove the surrounding whitespace and display the cleaned string.

---

## Section 5 — Collections and Mutability

### Exercise 21: List Modification
Create a list of five numbers. Change the first element, append another number, and remove one value. Print the final list.

### Exercise 22: Tuple Access
Create a tuple of five cities. Print the first, third, and last elements. Explain why assigning to a tuple index fails.

### Exercise 23: Dictionary Update
Create a student dictionary. Add a department, update the student's marks, and print the result.

### Exercise 24: Set Operations
Create two sets of programming languages. Calculate their union, intersection, and difference.

### Exercise 25: Mutable Objects
Assign a list to two variables. Modify the list through the second variable and inspect the first. Explain the result.

---

## Section 6 — Intermediate Challenges

### Exercise 26: Multiple Unpacking
Unpack a list of three values into three variables. Swap the first and third values.

### Exercise 27: Variable Scope
Create a global variable and a function that defines a local variable with the same name. Print the value inside and outside the function and explain the difference.

### Exercise 28: Data Type Inspector
Accept a value as text and ask the user to interpret it as an integer, float, or string. Convert it safely and report invalid numeric input.

### Exercise 29: Student Marks Analyzer
Store marks for five subjects in a list. Calculate the total, average, highest, and lowest marks.

### Exercise 30: Product Price Calculator
Store a product name, unit price, quantity, and discount percentage. Calculate the subtotal, discount, and final amount. Validate non-negative price and quantity and a discount between 0 and 100.

### Exercise 31: Employee Salary Record
Create a dictionary containing an employee's name, ID, basic salary, and allowance. Calculate and display gross salary. State your calculation assumptions.

### Exercise 32: Data Conversion Pipeline
Convert this list of numeric strings to integers, calculate the average, and create a list of squared values.

```python
values = ["10", "20", "30", "40", "50"]
```

### Exercise 33: Swap Dictionary Values
Create a dictionary with two keys. Exchange the values associated with those keys without changing the keys.

### Exercise 34: Data Validation
Accept a student's name, age, and marks. Validate that the name is not blank, age is a positive integer, and marks are between 0 and 100. Display a record only if the inputs are valid.

### Exercise 35: Customer Data Record
Represent a customer using a dictionary containing name, age, city, membership status, and purchase history. Calculate the total purchase amount and print a summary.

---

## Mini-Project Extensions

1. **Student Information System:** Add, update, search, and display student records.
2. **Shopping Bill Generator:** Calculate subtotals, discounts, and final totals.
3. **Employee Record Generator:** Store employee information and calculate gross salary.
4. **Data Type Inspector:** Inspect values and safely convert user input.
5. **Simple Dataset Summary:** Calculate count, sum, average, minimum, and maximum for a list of measurements.

## Submission Guidelines

- Include the exercise number and title in each solution.
- Add comments where they improve clarity.
- Test normal and invalid inputs.
- Record expected output or sample test cases.
- Save code in `practice.ipynb` or individual `.py` files.
- Commit completed work to GitHub.

## Self-Assessment

- [ ] I understand assignment and naming rules.
- [ ] I can identify Python data types.
- [ ] I can convert values safely.
- [ ] I can manipulate strings and collections.
- [ ] I understand mutability and references.
- [ ] I can validate user input.
- [ ] I can represent simple real-world records using Python.
