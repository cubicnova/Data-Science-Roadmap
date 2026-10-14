# NumPy Array Operations — Reference Solutions

Use `import numpy as np`. Alternative correct solutions are acceptable.

## A. Arithmetic and universal functions

1.
```python
a = np.array([2, 4, 6])
b = np.array([1, 3, 5])
print(a + b, a - b, a * b, a / b)
```
2. `a ** 2`
3. `np.sqrt(np.array([1, 4, 9, 16]))` → `[1. 2. 3. 4.]`
4. `np.abs(np.array([-4, -1, 0, 3]))` → `[4 1 0 3]`
5. `np.exp([0.0, 1.0])` and `np.log([1.0, np.e])`; use NumPy arrays for consistent vectorized behavior.
6. `angles = np.array([0, np.pi/2, np.pi]); np.sin(angles); np.cos(angles)`
7. `*` multiplies corresponding elements; `@` performs matrix multiplication with row/column summation.
8. `a > b`
9. `a[(a >= 5) & (a <= 15)]`
10. `np.where(a < 50, 0, a)`

## B. Aggregations and axes

11. `np.sum(x)`, `np.mean(x)`, `np.min(x)`, `np.max(x)`, `np.var(x)`, `np.std(x)`
12. `m.sum(axis=0)`
13. `m.mean(axis=1)`
14. `row_means = m.mean(axis=1, keepdims=True)`; shape is `(number_of_rows, 1)`.
15. `np.argmax(a)` and `np.argmin(a)`
16. `np.median(a)`
17. `ddof=0` uses the population-style denominator \(N\); `ddof=1` uses \(N-1\), the usual sample estimate when appropriate.

## C. Broadcasting and shape operations

18.
```python
a = np.array([[1], [2], [3]])
b = np.array([[10, 20, 30, 40]])
print(a + b)
print((a + b).shape)  # (3, 4)
```
19. `m + 5`
20. Compare trailing dimensions: `(3, 2)` and `(3,)` compare as `(3, 2)` and `(1, 3)`. The trailing sizes 2 and 3 differ and neither is 1.
21. `np.arange(12).reshape(3, 4).T`
22. `ravel()` may return a view if possible; `flatten()` always returns a copy.
23.
```python
def normalize(x):
    x = np.asarray(x, dtype=float)
    span = x.max() - x.min()
    if span == 0:
        return np.zeros_like(x)
    return (x - x.min()) / span
```

## D. Sorting, combining, and splitting

24. `np.sort(a)` for ascending; `np.sort(a)[::-1]` for descending.
25. `np.unique(a)` and `np.argsort(a)`
26. `np.where(a > threshold)`
27. `np.concatenate([a, b])`
28. `np.stack([a, b], axis=0)`
29. `np.split(np.arange(12), 4)`
30. `array_split` permits sections that differ in size when the array cannot be divided evenly.

## E. Matrix operations and mini tasks

31. `A @ B`
32.
```python
A = np.array([[2, 1], [1, 3]], dtype=float)
b = np.array([5, 6], dtype=float)
print(np.linalg.solve(A, b))  # [1.8 1.4]
```
33. For `A = np.array([[1, 2], [3, 4]])`, `np.linalg.det(A)` is approximately `-2.0`.
34.
```python
def summarize(x):
    x = np.asarray(x)
    return {
        "mean": float(np.mean(x)),
        "min": float(np.min(x)),
        "max": float(np.max(x)),
        "std": float(np.std(x)),
    }
```
35.
```python
names = np.array(["Asha", "Ravi", "Meena", "Dev"])
marks = np.array([68, 74, 92, 99])
adjusted = np.minimum(marks + 5, 100)
print("adjusted:", adjusted)
print("mean:", adjusted.mean())
print("at least 75:", names[adjusted >= 75])
```
The `np.minimum` operation caps each adjusted score at 100 without a loop.
