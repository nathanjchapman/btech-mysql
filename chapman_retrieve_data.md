# Retrieve Data Assignment

For each of the questions in this assignment include the SQL query and the results of the query. Use the database and tables that you created in the database creation and insert data modules.

### 1. Display the name and sex of all the students in the database organized in alphabetical order by name.

SQL:
```sql
SELECT name, sex FROM student ORDER BY name;
```

Output:
```
+-----------+-----+
| name      | sex |
+-----------+-----+
| Abby      | F   |
| Aubrey    | F   |
| Avery     | F   |
| Becca     | F   |
| Ben       | M   |
| Carter    | M   |
| Colin     | M   |
| Devri     | F   |
| Emily     | F   |
| Gabrielle | F   |
| Grace     | F   |
| Gregory   | M   |
| Ian       | M   |
| Joseph    | M   |
| Katie     | F   |
| Keaton    | M   |
| Kyle      | M   |
| Lauren    | F   |
| Liesl     | F   |
| Max       | M   |
| Megan     | F   |
| Michael   | M   |
| Nathan    | M   |
| Peter     | M   |
| Rebecca   | F   |
| Rianne    | F   |
| Robbie    | M   |
| Sarah     | F   |
| Teddy     | M   |
| Thomas    | M   |
| Will      | M   |
+-----------+-----+
```


### 2. Find all students whose names start with the letter 'K'.

SQL:
```sql
SELECT name FROM student WHERE name LIKE 'K%';
```

Output:
```
+--------+
| name   |
+--------+
| Kyle   |
| Katie  |
| Keaton |
+--------+
```

### 3. How many absences have been recorded in the absence table?

SQL:
```sql
SELECT COUNT(*) AS 'total_absences' FROM absence;
```

Output:
```
+----------------+
| total_absences |
+----------------+
|              5 |
+----------------+
```

### 4. How many quizzes have there been? This information is in the grade_event table.

SQL:
```sql
SELECT COUNT(*) AS 'number_of_quizes' FROM grade_event WHERE category LIKE 'Q';
```

Output:
```
+------------------+
| number_of_quizes |
+------------------+
|                4 |
+------------------+
```

### 5. Display all the scores for student id 27.

SQL:
```sql
SELECT name, score FROM student, score WHERE student.student_id = score.student_id AND student.student_id = 27;
```

Output:
```
+--------+-------+
| name   | score |
+--------+-------+
| Carter |    15 |
| Carter |     8 |
| Carter |    90 |
| Carter |    70 |
+--------+-------+
```

### 6. Display the student id and average score for student id 27.

SQL:
```sql
SELECT student.student_id, name, AVG(score) AS 'average_score' FROM student, score WHERE student.student_id = score.student_id AND student.student_id = 27;
```

Output:
```
+------------+--------+---------------+
| student_id | name   | average_score |
+------------+--------+---------------+
|         27 | Carter |       45.7500 |
+------------+--------+---------------+
```

### 7. Display all the grade events organized by date from most recent to least recent.

SQL:
```sql
SELECT * FROM grade_event ORDER BY `date` DESC;
```

Output:
```
+----------+------------+----------+
| event_id | date       | category |
+----------+------------+----------+
|        6 | 2012-10-01 | T        |
|        5 | 2012-09-23 | Q        |
|        4 | 2012-09-16 | Q        |
|        3 | 2012-09-09 | T        |
|        2 | 2012-09-06 | Q        |
|        1 | 2012-09-03 | Q        |
+----------+------------+----------+
```

### 8. Display all the scores that are greater than 75.

SQL:
```sql
SELECT name, score FROM score, student WHERE student.student_id=score.student_id AND score > 75 ORDER BY score DESC;
```

Output:
```
+---------+-------+
| name    | score |
+---------+-------+
| Megan   |   100 |
| Rebecca |    98 |
| Michael |    98 |
| Abby    |    97 |
| Abby    |    97 |
| Max     |    96 |
| Becca   |    95 |
| Max     |    94 |
| Will    |    94 |
| Kyle    |    94 |
| Keaton  |    91 |
| Lauren  |    91 |
| Joseph  |    91 |
| Carter  |    90 |
| Nathan  |    89 |
| Liesl   |    88 |
| Megan   |    88 |
| Keaton  |    86 |
| Robbie  |    85 |
| Joseph  |    84 |
| Nathan  |    83 |
| Colin   |    83 |
| Emily   |    81 |
| Becca   |    81 |
| Gregory |    81 |
| Robbie  |    79 |
| Will    |    79 |
| Rianne  |    79 |
| Grace   |    79 |
| Thomas  |    77 |
| Teddy   |    77 |
| Ben     |    77 |
| Emily   |    76 |
| Liesl   |    76 |
| Avery   |    76 |
+---------+-------+
```

### 9. Display all the scores less than 15.

SQL:
```sql
SELECT name, score FROM score, student WHERE student.student_id=score.student_id AND score < 15 ORDER BY score DESC;
```

Output:
```
+---------+-------+
| name    | score |
+---------+-------+
| Robbie  |    14 |
| Ian     |    14 |
| Liesl   |    14 |
| Peter   |    14 |
| Liesl   |    14 |
| Avery   |    14 |
| Will    |    13 |
| Nathan  |    13 |
| Becca   |    13 |
| Aubrey  |    13 |
| Thomas  |    13 |
| Teddy   |    13 |
| Katie   |    13 |
| Lauren  |    13 |
| Colin   |    13 |
| Abby    |    13 |
| Gregory |    13 |
| Kyle    |    13 |
| Abby    |    13 |
| Lauren  |    13 |
| Sarah   |    12 |
| Joseph  |    12 |
| Grace   |    12 |
| Lauren  |    12 |
| Peter   |    12 |
| Ian     |    12 |
| Emily   |    11 |
| Colin   |    11 |
| Will    |    11 |
| Ben     |    11 |
| Rianne  |    11 |
| Ben     |    11 |
| Grace   |    11 |
| Max     |    11 |
| Sarah   |    11 |
| Kyle    |    11 |
| Keaton  |    10 |
| Becca   |    10 |
| Robbie  |    10 |
| Robbie  |    10 |
| Rianne  |     9 |
| Emily   |     9 |
| Nathan  |     9 |
| Rebecca |     9 |
| Max     |     9 |
| Avery   |     9 |
| Will    |     9 |
| Aubrey  |     9 |
| Devri   |     8 |
| Thomas  |     8 |
| Ian     |     8 |
| Keaton  |     8 |
| Devri   |     8 |
| Joseph  |     8 |
| Carter  |     8 |
| Joseph  |     7 |
+---------+-------+
```

### 10. Display all the scores between 15 and 75.

SQL:
```sql
SELECT name, score FROM score, student WHERE student.student_id=score.student_id AND score BETWEEN 15 AND 75 ORDER BY score DESC;
```

Output:
```
+-----------+-------+
| name      | score |
+-----------+-------+
| Ian       |    75 |
| Aubrey    |    75 |
| Thomas    |    75 |
| Katie     |    74 |
| Michael   |    74 |
| Rianne    |    74 |
| Lauren    |    73 |
| Colin     |    73 |
| Peter     |    72 |
| Katie     |    71 |
| Carter    |    70 |
| Kyle      |    69 |
| Ben       |    68 |
| Teddy     |    68 |
| Sarah     |    68 |
| Grace     |    68 |
| Devri     |    67 |
| Gabrielle |    66 |
| Gabrielle |    66 |
| Ian       |    65 |
| Peter     |    63 |
| Sarah     |    62 |
| Aubrey    |    62 |
| Avery     |    62 |
| Rebecca   |    60 |
| Rebecca   |    20 |
| Becca     |    20 |
| Kyle      |    20 |
| Aubrey    |    20 |
| Megan     |    20 |
| Abby      |    20 |
| Max       |    20 |
| Teddy     |    20 |
| Emily     |    19 |
| Gabrielle |    19 |
| Thomas    |    19 |
| Emily     |    19 |
| Thomas    |    19 |
| Robbie    |    19 |
| Sarah     |    19 |
| Peter     |    19 |
| Liesl     |    19 |
| Colin     |    19 |
| Michael   |    18 |
| Peter     |    18 |
| Max       |    18 |
| Devri     |    18 |
| Nathan    |    18 |
| Katie     |    18 |
| Ian       |    18 |
| Rianne    |    18 |
| Rebecca   |    18 |
| Ben       |    18 |
| Keaton    |    18 |
| Michael   |    18 |
| Nathan    |    18 |
| Kyle      |    17 |
| Gregory   |    17 |
| Colin     |    17 |
| Avery     |    17 |
| Abby      |    17 |
| Liesl     |    17 |
| Grace     |    17 |
| Teddy     |    17 |
| Becca     |    17 |
| Lauren    |    17 |
| Megan     |    17 |
| Ben       |    16 |
| Gabrielle |    16 |
| Michael   |    16 |
| Katie     |    16 |
| Aubrey    |    16 |
| Gregory   |    16 |
| Gabrielle |    16 |
| Teddy     |    15 |
| Megan     |    15 |
| Keaton    |    15 |
| Michael   |    15 |
| Rianne    |    15 |
| Carter    |    15 |
| Gregory   |    15 |
| Rebecca   |    15 |
+-----------+-------+
```
