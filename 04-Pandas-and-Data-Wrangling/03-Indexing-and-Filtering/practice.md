# Practice Exercises — Pandas Indexing and Filtering

Complete these 35 exercises. Use the sample DataFrame below or create your own equivalent dataset.

```python
import pandas as pd

df = pd.DataFrame({
    'name': ['Asha', 'Dev', 'Mira', 'Ravi'],
    'age': [21, 24, 20, 27],
    'score': [78, 91, 85, 62],
    'city': ['Indore', 'Pune', 'Indore', 'Bhopal']
})
```

1. **Select a column:** Select the `score` column from a DataFrame.

2. **Select multiple columns:** Return a DataFrame containing only `name` and `city`.

3. **Use loc for a label:** Select the row with index label `2` and column `name`.

4. **Use iloc for a position:** Select the value in the third row and second column.

5. **Select first rows:** Use `.iloc` to select the first three rows.

6. **Select a column range:** Use `.iloc` to select the first two columns from all rows.

7. **Label slice:** Use `.loc` to select index labels 1 through 3, inclusive.

8. **Filter with one condition:** Select rows where `score` is at least 80.

9. **Filter with AND:** Select rows where `age` is at least 21 and `city` is `Indore`.

10. **Filter with OR:** Select rows where `city` is `Pune` or `Bhopal`.

11. **Negate a condition:** Select rows where the city is not `Indore`.

12. **Membership with isin:** Select rows whose city is in `['Indore', 'Pune']`.

13. **Filter a range:** Select rows where age is between 20 and 25, inclusive.

14. **Filter missing values:** Select rows where `score` is missing.

15. **Filter non-missing values:** Select rows where `score` is not missing.

16. **Use query:** Use `query()` to select rows with age greater than 22 and score above 70.

17. **Use where:** Keep scores greater than or equal to 80 and replace other scores with missing values.

18. **Filter by two numeric conditions:** Select rows where score is at least 80 and age is below 25.

19. **Use a named mask:** Create a Boolean mask for score below 70, then use it to filter rows.

20. **Count matches:** Count how many rows have a score of at least 80.

21. **Select a single row:** Select the final row using `.iloc`.

22. **Select a row by custom index:** Set `name` as the index and select the row labeled `Mira`.

23. **Reset an index:** Reset a DataFrame index after setting `name` as its index.

24. **Check index uniqueness:** Print whether a DataFrame's index is unique.

25. **Filter by list of scores:** Select rows whose score is one of 62, 78, or 91.

26. **Combine three conditions:** Select rows where age is at least 21, score is at least 75, and city is not `Bhopal`.

27. **Safely update rows:** Use `.loc` to set scores below 70 to 70.

28. **Copy a filtered subset:** Create an independent copy of rows with score at least 80.

29. **Filter string values:** Select names that start with `M` using `.str.startswith`.

30. **Case-insensitive string match:** Select rows whose city contains `ind` case-insensitively.

31. **Sort after filtering:** Filter scores at least 70 and sort by score descending.

32. **Filter with missing values:** Select rows where score is missing or score is greater than 85.

33. **Select rows and columns together:** Use `.loc` to filter score at least 80 and return only `name` and `score`.

34. **Mini challenge: employee records:** Filter employees in IT with salary greater than 60000, returning only name and salary.

35. **Mini challenge: student report:** Filter passing students with score at least 40, sort descending by score, and return the top two.

