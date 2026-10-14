# Solutions — NumPy Reshaping and Joining

Run the examples in a Python environment with NumPy installed. These are reference solutions; equivalent correct approaches are acceptable.

## 1. Inspect shape metadata

```python
import numpy as np
a = np.arange(12)
print(a.shape, a.ndim, a.size)
```

## 2. Reshape to a matrix

```python
np.arange(12).reshape(3, 4)
```

## 3. Infer columns

```python
np.arange(18).reshape(3, -1)
```

## 4. Infer rows

```python
np.arange(20).reshape(-1, 4)
```

## 5. Invalid reshape

```python
np.arange(10).reshape(3, 4)  # ValueError: 10 elements cannot fill 12 positions
```

## 6. Reshape a 3-D array

```python
np.arange(24).reshape(2, 3, 4)
```

## 7. Flatten with flatten

```python
np.arange(6).reshape(2, 3).flatten()
```

## 8. Flatten with ravel

```python
np.arange(6).reshape(2, 3).ravel()
```

## 9. Compare copy behavior

```python
a = np.arange(6).reshape(2, 3)
b = a.flatten(); b[0] = 99
print(a)  # unchanged
```

## 10. Check ravel sharing

```python
a = np.arange(6).reshape(2, 3)
b = a.ravel()
print(np.shares_memory(a, b))
```

## 11. Row vector

```python
np.arange(5)[np.newaxis, :]
```

## 12. Column vector

```python
np.arange(5)[:, np.newaxis]
```

## 13. Expand dimensions

```python
np.expand_dims(np.arange(3), axis=1)
```

## 14. Squeeze singleton axes

```python
np.squeeze(np.zeros((1, 3, 1))).shape
```

## 15. Squeeze a specific axis

```python
np.squeeze(np.zeros((1, 2, 1)), axis=0).shape
```

## 16. Concatenate rows

```python
np.concatenate((np.array([[1, 2], [3, 4]]), np.array([[5, 6]])), axis=0)
```

## 17. Concatenate columns

```python
np.concatenate((np.array([[1, 2], [3, 4]]), np.array([[5], [6]])), axis=1)
```

## 18. Concatenate 1-D arrays

```python
np.concatenate(([1, 2], [3, 4, 5]))
```

## 19. Stack arrays vertically

```python
np.vstack(([1, 2], [3, 4]))
```

## 20. Stack arrays horizontally

```python
np.hstack(([1, 2], [3, 4]))
```

## 21. Create a stack

```python
np.stack((np.array([1, 2]), np.array([3, 4])), axis=0)
```

## 22. Stack on another axis

```python
np.stack((np.array([1, 2, 3]), np.array([4, 5, 6])), axis=1)
```

## 23. Compare stack and concatenate

```python
print(np.stack(([1, 2], [3, 4]), axis=0)); print(np.concatenate(([1, 2], [3, 4]), axis=0))
```

## 24. Depth stack

```python
np.dstack((np.ones((2, 2)), np.zeros((2, 2)))).shape
```

## 25. Split into equal parts

```python
np.split(np.arange(12), 3)
```

## 26. Split into uneven parts

```python
np.array_split(np.arange(10), 3)
```

## 27. Split matrix rows

```python
np.vsplit(np.arange(12).reshape(6, 2), 3)
```

## 28. Split matrix columns

```python
np.hsplit(np.arange(12).reshape(2, 6), 3)
```

## 29. Choose an axis

```python
np.array_split(np.arange(12).reshape(3, 4), 3, axis=1)  # each section has shape (3, 1) or equivalent
```

## 30. Diagnose join mismatch

```python
np.concatenate((np.zeros((2, 3)), np.ones((2, 2))), axis=0)  # ValueError: widths differ
```

## 31. Join matching dimensions

```python
np.concatenate((np.zeros((2, 3)), np.ones((2, 2))), axis=1).shape  # (2, 5)
```

## 32. Join unequal rows

```python
np.concatenate((np.zeros((1, 3)), np.ones((2, 3))), axis=0).shape  # (3, 3)
```

## 33. Split and recombine

```python
a = np.arange(12)
parts = np.split(a, 4)
np.concatenate(parts)
```

## 34. Reshape and join

```python
np.stack((np.arange(6).reshape(2, 3), np.arange(6, 12).reshape(2, 3)), axis=0).shape  # (2, 2, 3)
```

## 35. Mini challenge: batch data

```python
a = np.arange(24).reshape(4, 6)
left, right = np.split(a, 2, axis=0)
print(a.shape, left.shape, right.shape)
```

