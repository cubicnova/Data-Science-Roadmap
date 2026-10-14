# Practice Exercises: Operators in Python

Complete the exercises in order. Try solving them independently before checking `solutions.md`. Use Python 3 and write readable code with descriptive variable names.

## Beginner: Arithmetic and Assignment

1. Create two integer variables and print their sum, difference, product, and quotient.
2. Calculate the remainder when `47` is divided by `6`.
3. Calculate both the true-division and floor-division results of `29` divided by `4`.
4. Calculate `3` raised to the power of `5`.
5. A product costs ₹275 and a customer buys 4 units. Calculate the total cost.
6. Convert `367` seconds into minutes and remaining seconds using `//` and `%`.
7. Start with `value = 12`. Use `+=`, `-=`, `*=`, and `/=` in sequence, printing the value after each operation.
8. Calculate the area of a rectangle with length `12.5` and width `4`.
9. Calculate the average of the numbers `12`, `18`, `25`, and `45`.
10. Given an integer, print whether it is even or odd using the modulus operator.

## Beginner: Comparisons and Logic

11. Compare two numbers and print whether the first is greater than, less than, or equal to the second.
12. Given `score = 76`, check whether the score is at least 40.
13. Check whether a temperature is between 18 and 30 inclusive using a chained comparison.
14. Given `age = 20` and `has_id = True`, check whether a person meets both entry requirements: age at least 18 and valid ID.
15. Given a username string, check whether it is empty using `not`.
16. Given `number = 15`, check whether it is greater than 10 and less than 20.
17. Given `day = "Saturday"`, check whether the day is `"Saturday"` or `"Sunday"`.
18. Write a condition that is true when a value is outside the inclusive range 1 to 100.
19. Explain in a comment why `and` may not evaluate its right-hand expression.
20. Given a list that may be empty, safely check whether its first item equals `"admin"` without causing an `IndexError`.

## Intermediate: Membership, Identity, and Precedence

21. Check whether `"Pandas"` appears in a list of data-science tools.
22. Given a dictionary with keys `"name"` and `"score"`, test whether `"score"` is a key and whether the value `90` is a key.
23. Create two separate lists with identical contents. Compare them with `==` and `is`, then explain the results.
24. Set a variable to `None` and check it using `is None`.
25. Predict and then calculate the result of `2 + 3 * 4`.
26. Use parentheses to make `(2 + 3) * 4` evaluate to `20`.
27. Calculate `2 ** 3 ** 2` and explain the associativity.
28. Rewrite a complicated Boolean condition with parentheses to make its intent clear.

## Intermediate: Bitwise Operators

29. Convert the integers `6` and `3` to binary using `bin()`, then calculate `6 & 3`, `6 | 3`, and `6 ^ 3`.
30. Calculate `3 << 2` and `16 >> 2`.
31. Define `READ = 1`, `WRITE = 2`, and `EXECUTE = 4`. Combine `READ` and `WRITE` using `|`, then test whether `WRITE` is present using `&`.
32. Explain the result of `~6` in Python.

## Applied Data Science

33. Given `scores = [82, 47, 35, 91, 68]`, create a list containing only scores of 40 or more.
34. Given `prices = [100, 250, 80, 500]`, create a list of prices greater than 100 and less than or equal to 300.
35. Given a list of numbers, calculate the percentage that are even. Handle an empty list safely so you do not divide by zero.

## Self-Assessment

For each exercise, be ready to explain:
- Which operator family you used.
- The expected data type of the result.
- Why the expression evaluates as it does.
- Any edge case that could affect the result.
