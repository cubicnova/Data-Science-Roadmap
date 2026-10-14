# Solutions — Pandas Importing and Exporting Data

Reference solutions for all exercises. Some examples create temporary files under `/tmp` and may be adjusted for Windows or your own project folder.

## 1. Read a CSV

```python
from pathlib import Path
import pandas as pd
p = Path('students.csv')
p.write_text('name,score\nAsha,80\nDev,90\n', encoding='utf-8')
pd.read_csv(p)
```

## 2. Inspect imported data

```python
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [80, 90]})
print(df.head(), df.shape, df.columns.tolist(), df.dtypes, sep='\n')
```

## 3. Select columns during import

```python
from io import StringIO
csv = StringIO('name,score,city\nAsha,80,Indore\nDev,90,Pune\n')
pd.read_csv(csv, usecols=['name', 'score'])
```

## 4. Read a CSV without headers

```python
from io import StringIO
pd.read_csv(StringIO('Asha,80\nDev,90\n'), header=None, names=['name', 'score'])
```

## 5. Change the delimiter

```python
from io import StringIO
pd.read_csv(StringIO('name;score\nAsha;80\nDev;90\n'), sep=';')
```

## 6. Set a dtype

```python
from io import StringIO
pd.read_csv(StringIO('id,name\n001,Asha\n002,Dev\n'), dtype={'id': 'string'})
```

## 7. Parse dates

```python
from io import StringIO
pd.read_csv(StringIO('name,date\nAsha,2026-01-02\nDev,2026-02-03\n'), parse_dates=['date']).dtypes
```

## 8. Custom missing markers

```python
from io import StringIO
pd.read_csv(StringIO('name,score\nAsha,NA\nDev,missing\n'), na_values=['NA', 'missing'])
```

## 9. Export CSV

```python
from pathlib import Path
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [80, 90]})
df.to_csv(Path('/tmp/pandas_export.csv'), index=False)
```

## 10. Export selected columns

```python
from pathlib import Path
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [80, 90], 'city': ['Indore', 'Pune']})
df.to_csv('/tmp/selected.csv', columns=['name', 'score'], index=False)
```

## 11. Control missing representation

```python
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [80.0, None]})
print(df.to_csv(index=False, na_rep='NA'))
```

## 12. Read Excel

```python
from pathlib import Path
p = Path('/tmp/marks.xlsx')
# Create a sample workbook, then read the Marks sheet
with pd.ExcelWriter(p) as writer:
    pd.DataFrame({'name': ['Asha'], 'score': [80]}).to_excel(writer, sheet_name='Marks', index=False)
pd.read_excel(p, sheet_name='Marks')
```

## 13. List Excel sheets

```python
from pathlib import Path
p = Path('/tmp/multi.xlsx')
with pd.ExcelWriter(p) as writer:
    pd.DataFrame({'x': [1]}).to_excel(writer, sheet_name='One', index=False)
    pd.DataFrame({'y': [2]}).to_excel(writer, sheet_name='Two', index=False)
result = pd.read_excel(p, sheet_name=None)
list(result.keys())
```

## 14. Write Excel

```python
from pathlib import Path
p = Path('/tmp/results.xlsx')
df = pd.DataFrame({'name': ['Asha'], 'score': [80]})
df.to_excel(p, sheet_name='Results', index=False)
```

## 15. Write multiple Excel sheets

```python
from pathlib import Path
p = Path('/tmp/two_sheets.xlsx')
with pd.ExcelWriter(p) as writer:
    pd.DataFrame({'name': ['Asha']}).to_excel(writer, sheet_name='Students', index=False)
    pd.DataFrame({'subject': ['Math']}).to_excel(writer, sheet_name='Subjects', index=False)
print(pd.ExcelFile(p).sheet_names)
```

## 16. Read JSON

```python
from io import StringIO
pd.read_json(StringIO('[{"name":"Asha","score":80},{"name":"Dev","score":90}]'))
```

## 17. Export JSON records

```python
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [80, 90]})
df.to_json(orient='records', indent=2)
```

## 18. Read tab-separated data

```python
from io import StringIO
pd.read_table(StringIO('name\tscore\nAsha\t80\nDev\t90\n'))
```

## 19. Normalize nested JSON

```python
records = [{'name': 'Asha', 'address': {'city': 'Indore', 'pin': 452001}}, {'name': 'Dev', 'address': {'city': 'Pune', 'pin': 411001}}]
pd.json_normalize(records)
```

## 20. Validate row counts

```python
df = pd.DataFrame({'name': ['Asha', 'Dev']})
expected_rows = 2
assert len(df) == expected_rows
print('row count validated:', len(df))
```

## 21. Find missing values

```python
df = pd.DataFrame({'a': [1, None, 3], 'b': [None, 'x', 'y']})
df.isna().sum()
```

## 22. Find duplicate rows

```python
df = pd.DataFrame({'x': [1, 1, 2], 'y': ['a', 'a', 'b']})
print('duplicates:', df.duplicated().sum())
print(df.drop_duplicates())
```

## 23. Convert imported numbers

```python
df = pd.DataFrame({'amount': ['10', '20', '30']})
df['amount'] = pd.to_numeric(df['amount'])
df.dtypes
```

## 24. Handle invalid numbers

```python
pd.to_numeric(pd.Series(['10', 'bad', '30']), errors='coerce')
```

## 25. Read only a few rows

```python
from io import StringIO
pd.read_csv(StringIO('x\n' + '\n'.join(map(str, range(150)))), nrows=100).shape
```

## 26. Process chunks

```python
from io import StringIO
source = StringIO('value\n' + '\n'.join(map(str, range(250))))
count = sum(len(chunk) for chunk in pd.read_csv(source, chunksize=100))
count
```

## 27. Encoding

```python
from io import StringIO
pd.read_csv(StringIO('name,city\nAsha,Indore\n'), encoding='utf-8')
```

## 28. Export tab-separated text

```python
df = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [80, 90]})
text = df.to_csv(sep='\t', index=False)
print(text)
```

## 29. Round-trip CSV

```python
from io import StringIO
original = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [80, 90]})
text = original.to_csv(index=False)
restored = pd.read_csv(StringIO(text))
print(restored.equals(original))
```

## 30. Round-trip JSON

```python
from io import StringIO
original = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [80, 90]})
text = original.to_json(orient='records')
restored = pd.read_json(StringIO(text), orient='records')
print(restored['name'].tolist(), restored.shape)
```

## 31. File existence check

```python
from pathlib import Path
p = Path('/tmp/example.csv')
p.write_text('x\n1\n', encoding='utf-8')
print(p.exists())
```

## 32. Clean before export

```python
df = pd.DataFrame({'name': ['Asha', 'Asha', 'Dev'], 'score': [80.0, 80.0, None]})
clean = df.drop_duplicates().copy()
clean['score'] = clean['score'].fillna(clean['score'].median())
clean.to_csv('/tmp/cleaned.csv', index=False)
clean
```

## 33. Export without index

```python
from io import StringIO
df = pd.DataFrame({'name': ['Asha', 'Dev']}, index=[101, 102])
print('with index:\n', df.to_csv())
print('without index:\n', df.to_csv(index=False))
```

## 34. Mini-project: data pipeline

```python
from pathlib import Path
source = Path('/tmp/students_pipeline.csv')
df = pd.DataFrame({'name': ['Asha', 'Dev', 'Mira'], 'score': [80, 90, None]})
df.to_csv(source, index=False)
loaded = pd.read_csv(source)
assert list(loaded.columns) == ['name', 'score'] and len(loaded) == 3
loaded['score'] = loaded['score'].fillna(loaded['score'].median())
loaded.to_csv('/tmp/students_clean.csv', index=False)
loaded
```

## 35. Mini-project: multi-format

```python
from pathlib import Path
source = pd.DataFrame({'name': ['Asha', 'Dev'], 'score': [80, 90]})
source.to_csv('/tmp/multi.csv', index=False)
source.to_json('/tmp/multi.json', orient='records')
csv_df = pd.read_csv('/tmp/multi.csv')
json_df = pd.read_json('/tmp/multi.json', orient='records')
print('CSV rows:', len(csv_df), 'JSON rows:', len(json_df))
```

