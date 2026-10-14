# Practice Exercises — Pandas Series and DataFrames

Complete the 35 exercises before reviewing `solutions.md`.

1. **Create a Series:** Create a Series from `[10, 20, 30, 40]`.

2. **Named Series:** Create a Series with values `[75, 88, 92]` and labels `Asha`, `Dev`, and `Mira`.

3. **Series name:** Create a Series named `temperature` from `[28, 31, 29]`.

4. **Series selection by label:** Using a custom-index Series, select the value labeled `Dev` with `.loc`.

5. **Series selection by position:** Select the second element of a Series with `.iloc`.

6. **Series summary:** Print the mean, minimum, and maximum of a numeric Series.

7. **Create a DataFrame:** Create a DataFrame with `name` and `age` columns for three people.

8. **From dictionary of lists:** Create a DataFrame with columns `product` and `price` from a dictionary.

9. **From list of dictionaries:** Create a DataFrame from two dictionaries representing students and scores.

10. **Inspect shape:** Print the shape of a DataFrame with 4 rows and 3 columns.

11. **Inspect columns and index:** Display the columns and index of a DataFrame.

12. **Preview rows:** Show the first two rows and last row of a DataFrame.

13. **Select one column:** Select the `score` column as a Series.

14. **Select multiple columns:** Select `name` and `score` as a DataFrame.

15. **Select by label:** Use `.loc` to select row label 1 and column `name`.

16. **Select by position:** Use `.iloc` to select the second row and third column.

17. **Filter rows:** Select rows where `score` is greater than or equal to 80.

18. **Add a column:** Add a Boolean `passed` column based on scores greater than or equal to 40.

19. **Rename a column:** Rename `score` to `marks`.

20. **Drop a column:** Remove the `passed` column from a DataFrame.

21. **Set an index:** Set the `name` column as the DataFrame index.

22. **Reset an index:** Reset an index created from the `name` column.

23. **Inspect dtypes:** Print the data types of all columns.

24. **Descriptive statistics:** Use `describe()` on a DataFrame containing numeric columns.

25. **Find missing values:** Create a DataFrame with one missing value and identify it using `isna()`.

26. **Count missing values:** Count missing values in each column.

27. **Fill missing values:** Fill missing values in a numeric column with that column's mean.

28. **Drop incomplete rows:** Remove rows that contain missing values using `dropna()`.

29. **Series alignment:** Add two Series with indexes `x, y` and `y, x` and inspect the aligned result.

30. **Convert a column type:** Convert a numeric-string column to numbers using `pd.to_numeric`.

31. **Sort rows:** Sort a DataFrame by `score` in descending order.

32. **Unique values:** Find the unique values in a Series and count each value.

33. **Copy a DataFrame:** Make an independent copy of a DataFrame and modify the copy.

34. **Student report:** Build a marks DataFrame and calculate each student's total and average.

35. **Mini challenge: data cleaning:** Create a small employee DataFrame with missing salary values, inspect missingness, fill salary with the median, and report final dtypes.

