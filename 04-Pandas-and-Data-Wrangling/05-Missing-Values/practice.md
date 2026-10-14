# Practice Exercises — Missing Values in Pandas

**Instructions:** Use Pandas and small self-created datasets. Keep a raw copy where appropriate and explain the reason for any imputation or deletion.

1. Create a DataFrame with at least three columns and some missing values.
2. Display a Boolean DataFrame showing where values are missing.
3. Count missing values in each column.
4. Calculate the total number of missing cells in a DataFrame.
5. Calculate the percentage of missing values in each column.
6. Select only rows where the age column is missing.
7. Select rows where any column contains a missing value.
8. Drop rows containing at least one missing value.
9. Drop rows only when the score column is missing.
10. Drop rows when either age or score is missing.
11. Drop columns that contain only missing values.
12. Keep only rows that have at least three non-missing values.
13. Fill missing city values with the string 'Unknown'.
14. Fill missing numeric values with the column mean.
15. Fill missing numeric values with the column median.
16. Fill missing categorical values with the mode.
17. Fill two different columns with different replacement values using a dictionary.
18. Use forward fill on an ordered Series with missing values.
19. Use backward fill on an ordered Series with missing values.
20. Explain why time-series data should be sorted before forward filling.
21. Replace the placeholders 'N/A' and '-' with actual missing values.
22. Read a CSV while treating 'N/A' and '-' as missing values (write the code).
23. Create a Boolean missingness indicator column for a score field.
24. Compare row count before and after calling dropna().
25. Find the column with the largest number of missing values.
26. Fill age using the median age within each city.
27. Add a global-median fallback for groups with no available median.
28. Demonstrate how filling a missing numeric value with zero can change the mean.
29. Explain one risk of dropping every row with any missing value.
30. Explain the difference between MCAR, MAR, and MNAR.
31. Show how to use notna() to select rows with a known email address.
32. Fill missing values in a dictionary-style call to DataFrame.fillna().
33. Build a cleaned DataFrame while leaving the original DataFrame unchanged.
34. After filling missing values, check whether any missing values remain.
35. Create a short missing-data cleaning pipeline and report missing counts before and after.
