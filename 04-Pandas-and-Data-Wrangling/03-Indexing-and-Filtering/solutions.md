# Solutions — Pandas Indexing and Filtering

Reference solutions for all exercises. Equivalent correct approaches are acceptable.

## 1. Select a column

```python
import pandas as pd

df = pd.DataFrame({'name':['Asha','Dev','Mira','Ravi'],'age':[21,24,20,27],'score':[78,91,85,62],'city':['Indore','Pune','Indore','Bhopal']})
df['score']
```

## 2. Select multiple columns

```python
df[['name', 'city']]
```

## 3. Use loc for a label

```python
df.loc[2, 'name']
```

## 4. Use iloc for a position

```python
df.iloc[2, 1]
```

## 5. Select first rows

```python
df.iloc[:3]
```

## 6. Select a column range

```python
df.iloc[:, :2]
```

## 7. Label slice

```python
df.loc[1:3]
```

## 8. Filter with one condition

```python
df[df['score'] >= 80]
```

## 9. Filter with AND

```python
df[(df['age'] >= 21) & (df['city'] == 'Indore')]
```

## 10. Filter with OR

```python
df[df['city'].isin(['Pune', 'Bhopal'])]
```

## 11. Negate a condition

```python
df[~(df['city'] == 'Indore')]
```

## 12. Membership with isin

```python
df[df['city'].isin(['Indore', 'Pune'])]
```

## 13. Filter a range

```python
df[df['age'].between(20, 25)]
```

## 14. Filter missing values

```python
df2 = df.copy(); df2.loc[1, 'score'] = None
df2[df2['score'].isna()]
```

## 15. Filter non-missing values

```python
df2 = df.copy(); df2.loc[1, 'score'] = None
df2[df2['score'].notna()]
```

## 16. Use query

```python
df.query('age > 22 and score > 70')
```

## 17. Use where

```python
df['score'].where(df['score'] >= 80)
```

## 18. Filter by two numeric conditions

```python
df[(df['score'] >= 80) & (df['age'] < 25)]
```

## 19. Use a named mask

```python
mask = df['score'] < 70
df[mask]
```

## 20. Count matches

```python
int((df['score'] >= 80).sum())
```

## 21. Select a single row

```python
df.iloc[-1]
```

## 22. Select a row by custom index

```python
df.set_index('name').loc['Mira']
```

## 23. Reset an index

```python
df.set_index('name').reset_index()
```

## 24. Check index uniqueness

```python
df.index.is_unique
```

## 25. Filter by list of scores

```python
df[df['score'].isin([62, 78, 91])]
```

## 26. Combine three conditions

```python
df[(df['age'] >= 21) & (df['score'] >= 75) & (df['city'] != 'Bhopal')]
```

## 27. Safely update rows

```python
df2 = df.copy()
df2.loc[df2['score'] < 70, 'score'] = 70
df2
```

## 28. Copy a filtered subset

```python
high = df.loc[df['score'] >= 80].copy()
high
```

## 29. Filter string values

```python
df[df['name'].str.startswith('M')]
```

## 30. Case-insensitive string match

```python
df[df['city'].str.contains('ind', case=False, na=False)]
```

## 31. Sort after filtering

```python
df.loc[df['score'] >= 70].sort_values('score', ascending=False)
```

## 32. Filter with missing values

```python
df2 = df.copy(); df2.loc[1, 'score'] = None
df2[df2['score'].isna() | (df2['score'] > 85)]
```

## 33. Select rows and columns together

```python
df.loc[df['score'] >= 80, ['name', 'score']]
```

## 34. Mini challenge: employee records

```python
employees = pd.DataFrame({'name':['Asha','Dev','Mira'],'department':['IT','HR','IT'],'salary':[70000,65000,55000]})
employees.loc[(employees['department']=='IT') & (employees['salary']>60000), ['name','salary']]
```

## 35. Mini challenge: student report

```python
students = pd.DataFrame({'name':['Asha','Dev','Mira','Ravi'],'score':[78,35,91,62]})
students.loc[students['score'] >= 40].sort_values('score', ascending=False).head(2)
```

