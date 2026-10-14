# Practice Exercises — Database Fundamentals

Use standard SQL unless the task requires a database-specific feature. For SQLite exercises, adapt data types as needed.

1. Explain the difference between a database and a DBMS.
2. Define table, row, column, schema, and query in your own words.
3. List two relational and two non-relational database systems.
4. Describe one situation where a relational database is a good fit.
5. Create a `departments` table with an integer primary key and unique, non-null department name.
6. Create a `students` table with a primary key, required name, and department foreign key.
7. Insert three department records.
8. Insert five student records across at least two departments.
9. Write a query that selects all student names and IDs.
10. Write a query that returns only students in department ID 2.
11. Explain why a primary key cannot contain duplicate or null values.
12. Identify a suitable primary key for an enrollment table where students can take multiple courses.
13. Explain the difference between a primary key and a foreign key.
14. Add a `credits` column to a courses table using an appropriate SQL data type.
15. Create a products table with a non-negative price constraint.
16. Explain the purpose of `NOT NULL`, `UNIQUE`, `DEFAULT`, and `CHECK` constraints.
17. Write an update statement that changes one course name using its course ID.
18. Write a delete statement that removes one student by student ID; explain why the WHERE clause matters.
19. Describe one-to-one, one-to-many, and many-to-many relationships.
20. Design a junction table for students and courses.
21. Explain first normal form (1NF) using an example of a multi-valued column.
22. Describe an update anomaly caused by storing department information redundantly with every student.
23. Explain what a transaction is and give an example involving two related updates.
24. Expand the ACID acronym and briefly define each property.
25. Write SQL to create an index on `students.department_id`.
26. Give one benefit and one cost of adding an index.
27. Explain why an application should use parameterized queries for user-supplied values.
28. Explain the difference between `DELETE`, `DROP TABLE`, and `TRUNCATE` at a high level.
29. Write a query that counts students in each department.
30. Write a query that joins students to departments and displays both names.
31. Explain what referential integrity prevents.
32. Suggest data types for age, price, date of birth, and a person's name.
33. Describe a safe process for changing a production database schema.
34. Sketch a normalized design for a library with books, members, and loans.
35. Build and query a small SQLite database containing departments, students, courses, and enrollments.
