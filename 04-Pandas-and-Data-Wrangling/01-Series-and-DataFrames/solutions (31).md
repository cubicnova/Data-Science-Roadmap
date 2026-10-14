# Solutions — Pandas Series and DataFrames

Reference answers for all exercises. Equivalent correct solutions are acceptable.

## 1. Create a Series

```python
import pandas as pd
pd.Series([10, 20, 30, 40])
```

## 2. Named Series

```python
pd.Series([75, 88, 92], index=['Asha', 'Dev', 'Mira'])
```

## 3. Series name

```python
pd.Series([28, 31, 29], name='temperature')
```

## 4. Series selection by label

```python
s = pd.Series([75, 88, 92], index=['Asha', 'Dev', 'Mira'])
s.loc['Dev']
```

## 5. Series selection by position

```python
s = pd.Series([75, 88, 92], index=['Asha', 'Dev', 'Mira'])
s.iloc[1]
```

## 6. Series summary

```python
s = pd.Series([10, 20, 30])
print(s.mean(), s.min(), s.max())
```

## 7. Create a DataFrame

```python
pd.DataFrame({'name': ['Asha', 'Dev', 'Mira'], 'age': [21, 22, 20]})
```

## 8. From dictionary of lists

```python
pd.DataFrame({'product': ['Pen', 'Book'], 'price': [10, 80]})
```

## 9. From list of dictionaries

```python
pd.DataFrame([{'student': 'Asha', 'score': 78}, {'student': 'Dev', 'score': 85}])
```

## 10. Inspect shape

```python
df = pd.DataFrame({'a': [1, 2, 3, 4], 'b': [5, 6, 7, 8], 'c': [9, 10, 11, 12]})
df.shape
```

## 11. Inspect columns and index

```python
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [78, 85]})
print(df.columns, df.index)
```

## 12. Preview rows

```python
df = pd.DataFrame({'name': ['Asha', 'Dev', 'Mira'], 'score': [78, 85, 92]})
print(df.head(2)); print(df.tail(1))
```

## 13. Select one column

```python
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [78, 85]})
df['score']
```

## 14. Select multiple columns

```python
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [78, 85]})
df[['name', 'score']]
```

## 15. Select by label

```python
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [78, 85]}, index=[0, 1])
df.loc[1, 'name']
```

## 16. Select by position

```python
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'age': [21, 22], 'score': [78, 85]})
df.iloc[1, 2]
```

## 17. Filter rows

```python
df = pd.DataFrame({'name': ['Asha', 'Dev', 'Mira'], 'score': [78, 85, 35]})
df[df['score'] >= 80]
```

## 18. Add a column

```python
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [78, 35]})
df['passed'] = df['score'] >= 40
print(df)
```

## 19. Rename a column

```python
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [78, 85]})
df.rename(columns={'score': 'marks'})
```

## 20. Drop a column

```python
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [78, 35], 'passed': [True, False]})
df.drop(columns=['passed'])
```

## 21. Set an index

```python
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [78, 85]})
df.set_index('name')
```

## 22. Reset an index

```python
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [78, 85]}).set_index('name')
df.reset_index()
```

## 23. Inspect dtypes

```python
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'age': [21, 22]})
df.dtypes
```

## 24. Descriptive statistics

```python
df = pd.DataFrame({'age': [20, 21, 22], 'score': [70, 80, 90]})
df.describe()
```

## 25. Find missing values

```python
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [78.0, float('nan')]})
df.isna()
```

## 26. Count missing values

```python
df = pd.DataFrame({'a': [1, None, 3], 'b': [None, 'x', 'y']})
df.isna().sum()
```

## 27. Fill missing values

```python
df = pd.DataFrame({'score': [70.0, None, 90.0]})
df['score'] = df['score'].fillna(df['score'].mean())
df
```

## 28. Drop incomplete rows

```python
df = pd.DataFrame({'name': ['Asha', 'Dev', 'Mira'], 'score': [70.0, None, 90.0]})
df.dropna()
```

## 29. Series alignment

```python
a = pd.Series([10, 20], index=['x', 'y'])
b = pd.Series([1, 2], index=['y', 'x'])
a + b
```

## 30. Convert a column type

```python
df = pd.DataFrame({'price': ['10', '20', '30']})
df['price'] = pd.to_numeric(df['price'])
print(df, df.dtypes)
```

## 31. Sort rows

```python
df = pd.DataFrame({'name': ['Asha', 'Dev', 'Mira'], 'score': [78, 92, 85]})
df.sort_values('score', ascending=False)
```

## 32. Unique values

```python
s = pd.Series(['A', 'B', 'A', 'C', 'B', 'A'])
print(s.unique())
print(s.value_counts())
```

## 33. Copy a DataFrame

```python
df = pd.DataFrame({'x': [1, 2]})
copy_df = df.copy()
copy_df.loc[0, 'x'] = 99
print('original:', df['x'].tolist(), 'copy:', copy_df['x'].tolist())
```

## 34. Student report

```python
df = pd.DataFrame({'name': ['Asha', 'Dev', 'Mira'], 'math': [80, 70, 90], 'science': [85, 75, 95]})
df['total'] = df[['math', 'science']].sum(axis=1)
df['average'] = df[['math', 'science']].mean(axis=1)
df
```

## 35. Mini challenge: data cleaning

```python
df = pd.DataFrame({'employee': ['A', 'B', 'C'], 'salary': [50000.0, None, 65000.0]})
print('missing before:', df.isna().sum().to_dict())
df['salary'] = df['salary'].fillna(df['salary'].median())
print(df)
print(df.dtypes)
```

