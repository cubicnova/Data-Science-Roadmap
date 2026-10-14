# Practice Exercises — Importing and Exporting Data with Pandas

Complete these 35 exercises. Most use small in-memory examples so you can practise without downloading external datasets.

1. **Read a CSV:** Create a small CSV file and load it using `pd.read_csv`.

2. **Inspect imported data:** After reading a CSV, display the first five rows, shape, columns, and dtypes.

3. **Select columns during import:** Use `usecols` to read only `name` and `score` from a CSV.

4. **Read a CSV without headers:** Read a CSV with no header row and assign column names using `names`.

5. **Change the delimiter:** Read a semicolon-separated text file using `sep=';'`.

6. **Set a dtype:** Read an ID column as string to preserve leading zeros.

7. **Parse dates:** Read a CSV and parse its `date` column as dates.

8. **Custom missing markers:** Treat strings `NA` and `missing` as missing values while reading a CSV.

9. **Export CSV:** Write a DataFrame to CSV without writing its index.

10. **Export selected columns:** Export only `name` and `score` to a CSV file.

11. **Control missing representation:** Export a DataFrame so missing values appear as `NA`.

12. **Read Excel:** Read the `Marks` worksheet from an Excel file.

13. **List Excel sheets:** Read all sheets from an Excel workbook into a dictionary of DataFrames.

14. **Write Excel:** Export a DataFrame to an Excel file with sheet name `Results` and no index.

15. **Write multiple Excel sheets:** Use `ExcelWriter` to write two DataFrames to separate sheets.

16. **Read JSON:** Read a JSON file containing records into a DataFrame.

17. **Export JSON records:** Export a DataFrame as JSON with `orient='records'` and indentation.

18. **Read tab-separated data:** Read a tab-separated text file using `pd.read_table`.

19. **Normalize nested JSON:** Use `pd.json_normalize` on a list of records containing nested fields.

20. **Validate row counts:** Compare an expected row count with the imported DataFrame's number of rows.

21. **Find missing values:** Count missing values per column after importing a dataset.

22. **Find duplicate rows:** Count duplicate rows and remove duplicates.

23. **Convert imported numbers:** Convert a numeric-looking string column to numeric values using `pd.to_numeric`.

24. **Handle invalid numbers:** Use `pd.to_numeric(errors='coerce')` to convert invalid numeric strings to missing values.

25. **Read only a few rows:** Read the first 100 rows from a large CSV using `nrows`.

26. **Process chunks:** Read a CSV in chunks of 100 rows and calculate the total number of rows processed.

27. **Encoding:** Read a UTF-8 CSV with an explicit `encoding` argument.

28. **Export tab-separated text:** Write a DataFrame as TSV using `sep='\t'`.

29. **Round-trip CSV:** Export a DataFrame to CSV and read it back; compare the DataFrames.

30. **Round-trip JSON:** Export a DataFrame as JSON records, read it back, and compare selected values.

31. **File existence check:** Use `pathlib.Path.exists()` to check whether a data file exists.

32. **Clean before export:** Drop duplicate rows, handle missing values, and export the cleaned data.

33. **Export without index:** Demonstrate the difference between CSV exports with and without the index.

34. **Mini-project: data pipeline:** Create a sample student dataset, export to CSV, import it, validate columns and row count, then save a cleaned CSV.

35. **Mini-project: multi-format:** Create one DataFrame, export it to CSV and JSON, read both files back, and compare their row counts.

