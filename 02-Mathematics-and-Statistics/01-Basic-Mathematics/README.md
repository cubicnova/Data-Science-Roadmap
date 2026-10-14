# Basic Mathematics for Data Science

Mathematics provides the language for representing data, describing relationships, and understanding machine-learning algorithms. This module reviews essential arithmetic, algebra, functions, exponents, logarithms, ratios, and summation notation used throughout data science.

## Learning objectives

- Work confidently with integers, fractions, percentages, ratios, and proportions.
- Simplify algebraic expressions and solve basic equations.
- Understand functions, coordinates, and slope.
- Use powers, roots, and logarithms.
- Interpret summation notation and simple formulas used in data analysis.
- Apply basic mathematical operations using Python.

## 1. Arithmetic and order of operations

Common operations include addition (`+`), subtraction (`-`), multiplication (`×`), division (`÷`), exponentiation, and parentheses.

Use the conventional order of operations: parentheses, exponents, multiplication/division, and addition/subtraction.

Example:

\[
3 + 4 \times 2 = 11
\]

\[
(3 + 4) \times 2 = 14
\]

In Python:

```python
result_a = 3 + 4 * 2
result_b = (3 + 4) * 2
print(result_a, result_b)
```

## 2. Fractions, decimals, and percentages

A fraction represents part of a whole. For example, \(3/4 = 0.75 = 75\%\).

Conversions:
- Decimal to percentage: multiply by 100.
- Percentage to decimal: divide by 100.
- Percentage of a quantity: quantity × percentage / 100.

Example: 15% of 240 is \(240 \times 15 / 100 = 36\).

Percentage change:

\[
\text{Percentage change} = \frac{\text{new} - \text{old}}{\text{old}} \times 100
\]

If a value rises from 80 to 100, the percentage change is 25%. Percentage change is undefined when the old value is zero.

## 3. Ratios and proportions

A ratio compares quantities, such as 2:3. A proportion states that two ratios are equal:

\[
\frac{a}{b} = \frac{c}{d}
\]

For nonzero denominators, cross multiplication gives \(ad = bc\). For example, if 4 notebooks cost ₹120, then 7 notebooks cost \(120 \times 7 / 4 = ₹210\), assuming a constant unit price.

## 4. Algebraic expressions

A variable represents a value. In \(3x + 5\), \(x\) is the variable, 3 is the coefficient, and 5 is the constant term.

Like terms can be combined:

\[
2x + 5x - 3 = 7x - 3
\]

Substitution evaluates an expression for a particular variable value. If \(x=4\), then \(3x+5=17\).

## 5. Solving linear equations

A linear equation in one variable can often be solved by isolating the variable while applying the same operation to both sides.

Example:

\[
3x + 5 = 20
\]

Subtract 5: \(3x=15\). Divide by 3: \(x=5\).

For an equation \(ax+b=c\), where \(a \ne 0\), the solution is \(x=(c-b)/a\).

## 6. Coordinates, functions, and slope

A coordinate pair \((x,y)\) identifies a point on a plane. A function maps an input to an output. For example, \(f(x)=2x+1\).

The slope between two points \((x_1,y_1)\) and \((x_2,y_2)\), when \(x_2 \ne x_1\), is:

\[
m = \frac{y_2-y_1}{x_2-x_1}
\]

Slope describes the change in \(y\) for a one-unit change in \(x\). This idea is important in linear models and regression.

## 7. Exponents and roots

An exponent represents repeated multiplication: \(2^3=2\times2\times2=8\).

Useful rules, where expressions are defined:
- \(a^m a^n = a^{m+n}\)
- \(a^m/a^n = a^{m-n}\) for \(a\ne0\)
- \((a^m)^n=a^{mn}\)
- \(a^0=1\) for \(a\ne0\)
- \(a^{-n}=1/a^n\) for \(a\ne0\)

A square root reverses squaring: \(\sqrt{25}=5\) for the principal nonnegative root.

## 8. Logarithms

A logarithm answers the question: “To what power must the base be raised to obtain this number?”

\[
\log_b(x)=y \iff b^y=x
\]

For real-valued logarithms, \(b>0\), \(b\ne1\), and \(x>0\).

Examples:
- \(\log_{10}(1000)=3\)
- \(\log_2(8)=3\)
- \(\ln(e^2)=2\)

Logarithms appear in log scales, information theory, probability, and many machine-learning objectives.

## 9. Summation and averages

The symbol \(\sum\) means “add a sequence of terms.” For example:

\[
\sum_{i=1}^{n} x_i = x_1+x_2+\cdots+x_n
\]

The arithmetic mean of \(n\) values is:

\[
\bar{x} = \frac{1}{n}\sum_{i=1}^{n}x_i
\]

For values 2, 4, and 9, the mean is \((2+4+9)/3=5\).

## 10. Absolute value and distance

Absolute value measures distance from zero:

\[
|x| =
\begin{cases}
x & x \ge 0\\
-x & x < 0
\end{cases}
\]

The distance between two real numbers \(a\) and \(b\) is \(|a-b|\). Absolute differences are useful in error measurement.

## 11. Mathematical operations with Python

```python
# Arithmetic
total = 12 + 8
quotient = 12 / 5
integer_quotient = 12 // 5
remainder = 12 % 5
power = 2 ** 5

# Percentage
percentage = 15
amount = 240
part = amount * percentage / 100

# Mean
values = [2, 4, 9]
mean = sum(values) / len(values)

print(total, quotient, integer_quotient, remainder, power)
print(part, mean)
```

Python's `/` performs true division, `//` performs floor division, `%` returns the remainder, and `**` raises a value to a power.

## 12. Best practices

- Use parentheses to make complicated expressions clear.
- Check units and the meaning of each quantity before calculating.
- Handle division by zero and logarithms of non-positive values carefully.
- Distinguish percentage points from percentage change.
- Keep adequate precision during intermediate calculations; round for display when appropriate.
- Verify results with estimation or an independent calculation.

## Quick reference

| Concept | Formula or example |
|---|---|
| Percentage of amount | `amount * percentage / 100` |
| Percentage change | `(new - old) / old * 100` |
| Linear equation | \(ax+b=c \Rightarrow x=(c-b)/a\), \(a\ne0\) |
| Slope | \((y_2-y_1)/(x_2-x_1)\) |
| Mean | \(\sum x_i/n\) |
| Absolute difference | \(|a-b|\) |
| Power in Python | `x ** n` |
| Logarithm | \(\log_b(x)=y \iff b^y=x\) |

## Summary

Basic mathematics supports data cleaning, descriptive statistics, visualization, and machine learning. Practice translating word problems into formulas, solving equations, and checking calculations programmatically.
