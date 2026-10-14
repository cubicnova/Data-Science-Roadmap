# Pandas Importing and Exporting Data

Pandas can read data from common file formats, inspect the result, and write cleaned or transformed data back to disk. This module focuses on CSV, Excel, JSON, and common import/export options.

## Learning objectives
- Read CSV, Excel, JSON, and delimited text files.
- Control headers, column names, indexes, data types, and missing-value markers.
- Inspect and validate imported data.
- Export DataFrames to common formats.
- Avoid common file path, encoding, and data-loss mistakes.

## 1. Setup and file paths

```python
import pandas as pd
from pathlib import Path
```

Use `Path` for readable cross-platform paths. Relative paths are resolved from the current working directory.

```python
path = Path("data") / "students.csv"
```

Check the current working directory and whether a file exists if a read operation fails.

## 2. CSV import and export

```python
df = pd.read_csv("students.csv")
df.to_csv("students_clean.csv", index=False)
```

Useful `read_csv` parameters:
- `sep`: delimiter, such as `","` or `"\t"`.
- `header`: row containing column names, or `None` when there is no header.
- `names`: explicit column names.
- `usecols`: read only selected columns.
- `dtype`: request column data types.
- `parse_dates`: parse date columns.
- `na_values`: additional strings interpreted as missing values.
- `encoding`: text encoding, such as `"utf-8"`.

Use `index=False` for exports when the DataFrame index is not part of the data you want to save.

## 3. Excel files

```python
df = pd.read_excel("students.xlsx", sheet_name="Marks")
df.to_excel("students_output.xlsx", sheet_name="Results", index=False)
```

Excel support commonly requires `openpyxl` for `.xlsx` files:

```bash
python -m pip install openpyxl
```

`sheet_name` may be a sheet name, sheet index, or `None` to read all sheets into a dictionary of DataFrames. When exporting multiple sheets, use `pd.ExcelWriter`.

## 4. JSON files

```python
df = pd.read_json("students.json")
df.to_json("students_output.json", orient="records", indent=2)
```

The correct `orient` depends on the JSON structure. For APIs, inspect a small sample first; nested JSON may need `pd.json_normalize`.

## 5. Other useful formats

- `pd.read_table(...)`: delimited text, tab-separated by default.
- `pd.read_parquet(...)`: Parquet columnar files (requires an appropriate engine such as `pyarrow`).
- `df.to_parquet(...)`: export to Parquet.
- `pd.read_pickle(...)` / `to_pickle(...)`: Python-specific serialization; only load pickle files from trusted sources.

Do not load untrusted pickle files because deserialization can execute code.

## 6. Validate imported data

After reading a file, inspect:

```python
df.head()
df.shape
df.columns
df.dtypes
df.isna().sum()
df.duplicated().sum()
```

Check expected row counts, column names, data types, missing values, duplicate records, and date parsing. A successful read does not guarantee that the data is correct.

## 7. Export options

```python
df.to_csv("out.csv", index=False, encoding="utf-8")
df.to_json("out.json", orient="records", indent=2)
```

Common options:
- `index=False`: avoid writing row labels as an extra column.
- `columns=[...]`: export only selected columns.
- `na_rep="NA"`: choose how missing values are represented.
- `encoding="utf-8"`: set text encoding.
- `date_format=...`: control date output format where supported.

## 8. Common errors and best practices

1. Verify the path and file extension.
2. Specify `sep` and `encoding` when automatic defaults do not match the source.
3. Use `index=False` for clean CSV/Excel exports unless the index is meaningful.
4. Check dtypes after import; numeric-looking strings may need conversion.
5. Validate dates and missing-value markers.
6. Avoid overwriting the original source before validating the transformed output.
7. Use trusted sources only for pickle files.
8. For large CSV files, consider `usecols`, `dtype`, and `chunksize`.

## Quick reference

| Format | Read | Write |
|---|---|---|
| CSV | `pd.read_csv(path)` | `df.to_csv(path, index=False)` |
| Excel | `pd.read_excel(path)` | `df.to_excel(path, index=False)` |
| JSON | `pd.read_json(path)` | `df.to_json(path, orient="records")` |
| Delimited text | `pd.read_table(path)` | `df.to_csv(path, sep="\\t", index=False)` |
| Parquet | `pd.read_parquet(path)` | `df.to_parquet(path, index=False)` |

## Further reading
- [Pandas IO tools](https://pandas.pydata.org/docs/user_guide/io.html)
- [read_csv](https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html)
- [read_excel](https://pandas.pydata.org/docs/reference/api/pandas.read_excel.html)
- [to_csv](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_csv.html)
