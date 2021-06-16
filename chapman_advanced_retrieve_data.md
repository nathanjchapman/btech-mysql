# Advanced Retrieve Data Assignment

For each of the questions in this assignment include the SQL query and the results of the query. Use the database and tables that you created in the database creation and insert data modules.

### 1. Using the LIKE operator, find all the students whose names begin with an "M".

SQL:
```sql
SELECT name FROM student WHERE name LIKE 'M%';
```

Output:
```
+---------+
| name    |
+---------+
| Megan   |
| Michael |
| Max     |
+---------+
```


### 2. Using the LIKE operator, find all the scores that end with a 1.

SQL:
```sql
SELECT score FROM score WHERE score LIKE '%1';
```

Output:
```
+-------+
| score |
+-------+
|    11 |
|    11 |
|    11 |
|    11 |
|    11 |
|    71 |
|    91 |
|    81 |
|    81 |
|    81 |
|    11 |
|    11 |
|    11 |
|    11 |
|    11 |
|    91 |
|    91 |
+-------+
```

### 3. Using the LIKE operator, find all students whose name ends with a "Y".

SQL:
```sql
SELECT name FROM student WHERE name LIKE '%Y';
```

Output:
```
+---------+
| name    |
+---------+
| Abby    |
| Aubrey  |
| Avery   |
| Gregory |
| Teddy   |
| Emily   |
+---------+
```

### 4. Using the IN and GROUP BY operator, find out how many students scored 7, 8, 9, and 10 points on their assignments. Display the count and the score.

SQL:
```sql
SELECT COUNT(*) AS 'number_of_students', score FROM student, score WHERE student.student_id=score.student_id AND score IN (7,8,9,10) GROUP BY score;
```

Output:
```
+--------------------+-------+
| number_of_students | score |
+--------------------+-------+
|                  1 |     7 |
|                  7 |     8 |
|                  8 |     9 |
|                  4 |    10 |
+--------------------+-------+
```

### 5. Using the GROUP BY operator, find the average score for each grade event in the grades table. Display the average and the event id.

SQL:
```sql
SELECT `date`, category, AVG(score.score) AS 'avg_score' FROM grade_event, score WHERE grade_event.event_id=score.event_id GROUP BY grade_event.event_id;
```

Output:
```
+------------+----------+-----------+
| date       | category | avg_score |
+------------+----------+-----------+
| 2012-09-03 | Q        |   15.1379 |
| 2012-09-06 | Q        |   14.1667 |
| 2012-09-09 | T        |   78.2258 |
| 2012-09-16 | Q        |   14.0370 |
| 2012-09-23 | Q        |   14.1852 |
| 2012-10-01 | T        |   80.1724 |
+------------+----------+-----------+

```

### 6. Using the IN and GROUP BY operators, find the average score for each student where the event id is not 3 or 6. Display the average and the student id.

SQL:
```sql
SELECT score.student_id, grade_event.event_id, AVG(score) AS 'avg_score' FROM score, grade_event WHERE grade_event.event_id NOT IN (3,6) GROUP BY student_id, event_id ORDER BY event_id;
```

Output:
```
+------------+----------+-----------+
| student_id | event_id | avg_score |
+------------+----------+-----------+
|          6 |        1 |   38.3333 |
|         15 |        1 |   32.5000 |
|         23 |        1 |   28.4000 |
|         31 |        1 |   35.8333 |
|          3 |        1 |   37.3333 |
|         11 |        1 |   39.8333 |
|         20 |        1 |   35.6000 |
|         28 |        1 |   35.0000 |
|          8 |        1 |   32.0000 |
|         17 |        1 |   41.2000 |
|         25 |        1 |   36.1667 |
|         13 |        1 |   25.2500 |
|          5 |        1 |   42.8333 |
|         14 |        1 |   33.5000 |
|         22 |        1 |   39.3333 |
|         30 |        1 |   37.4000 |
|          1 |        1 |   48.0000 |
|         10 |        1 |   33.0000 |
|         19 |        1 |   34.3333 |
|         27 |        1 |   45.7500 |
|          7 |        1 |   38.0000 |
|         16 |        1 |   36.6667 |
|         24 |        1 |   34.4000 |
|          2 |        1 |   40.4000 |
|          4 |        1 |   38.4000 |
|         12 |        1 |   35.1667 |
|         21 |        1 |   36.5000 |
|         29 |        1 |   36.6000 |
|          9 |        1 |   36.0000 |
|         18 |        1 |   41.3333 |
|         26 |        1 |   38.0000 |
|          4 |        2 |   38.4000 |
|         12 |        2 |   35.1667 |
|         21 |        2 |   36.5000 |
|         29 |        2 |   36.6000 |
|          9 |        2 |   36.0000 |
|         18 |        2 |   41.3333 |
|         26 |        2 |   38.0000 |
|          6 |        2 |   38.3333 |
|         15 |        2 |   32.5000 |
|         23 |        2 |   28.4000 |
|         31 |        2 |   35.8333 |
|          3 |        2 |   37.3333 |
|         11 |        2 |   39.8333 |
|         20 |        2 |   35.6000 |
|         28 |        2 |   35.0000 |
|          8 |        2 |   32.0000 |
|         17 |        2 |   41.2000 |
|         25 |        2 |   36.1667 |
|         13 |        2 |   25.2500 |
|          5 |        2 |   42.8333 |
|         14 |        2 |   33.5000 |
|         22 |        2 |   39.3333 |
|         30 |        2 |   37.4000 |
|          1 |        2 |   48.0000 |
|         10 |        2 |   33.0000 |
|         19 |        2 |   34.3333 |
|         27 |        2 |   45.7500 |
|          7 |        2 |   38.0000 |
|         16 |        2 |   36.6667 |
|         24 |        2 |   34.4000 |
|          2 |        2 |   40.4000 |
|          7 |        4 |   38.0000 |
|         16 |        4 |   36.6667 |
|         24 |        4 |   34.4000 |
|          2 |        4 |   40.4000 |
|          4 |        4 |   38.4000 |
|         12 |        4 |   35.1667 |
|         21 |        4 |   36.5000 |
|         29 |        4 |   36.6000 |
|          9 |        4 |   36.0000 |
|         18 |        4 |   41.3333 |
|         26 |        4 |   38.0000 |
|          6 |        4 |   38.3333 |
|         15 |        4 |   32.5000 |
|         23 |        4 |   28.4000 |
|         31 |        4 |   35.8333 |
|          3 |        4 |   37.3333 |
|         11 |        4 |   39.8333 |
|         20 |        4 |   35.6000 |
|         28 |        4 |   35.0000 |
|          8 |        4 |   32.0000 |
|         17 |        4 |   41.2000 |
|         25 |        4 |   36.1667 |
|         13 |        4 |   25.2500 |
|          5 |        4 |   42.8333 |
|         14 |        4 |   33.5000 |
|         22 |        4 |   39.3333 |
|         30 |        4 |   37.4000 |
|          1 |        4 |   48.0000 |
|         10 |        4 |   33.0000 |
|         19 |        4 |   34.3333 |
|         27 |        4 |   45.7500 |
|          1 |        5 |   48.0000 |
|         10 |        5 |   33.0000 |
|         19 |        5 |   34.3333 |
|         27 |        5 |   45.7500 |
|          7 |        5 |   38.0000 |
|         16 |        5 |   36.6667 |
|         24 |        5 |   34.4000 |
|          2 |        5 |   40.4000 |
|          4 |        5 |   38.4000 |
|         12 |        5 |   35.1667 |
|         21 |        5 |   36.5000 |
|         29 |        5 |   36.6000 |
|          9 |        5 |   36.0000 |
|         18 |        5 |   41.3333 |
|         26 |        5 |   38.0000 |
|          6 |        5 |   38.3333 |
|         15 |        5 |   32.5000 |
|         23 |        5 |   28.4000 |
|         31 |        5 |   35.8333 |
|          3 |        5 |   37.3333 |
|         11 |        5 |   39.8333 |
|         20 |        5 |   35.6000 |
|         28 |        5 |   35.0000 |
|          8 |        5 |   32.0000 |
|         17 |        5 |   41.2000 |
|         25 |        5 |   36.1667 |
|         13 |        5 |   25.2500 |
|          5 |        5 |   42.8333 |
|         14 |        5 |   33.5000 |
|         22 |        5 |   39.3333 |
|         30 |        5 |   37.4000 |
+------------+----------+-----------+
```

### 7. Using the GROUP BY operator, count the number of male and female students. Display sex and count.

SQL:
```sql
SELECT COUNT(*) AS `number_of_students`, sex FROM student GROUP BY sex;
```

Output:
```
+--------------------+-----+
| number_of_students | sex |
+--------------------+-----+
|                 16 | M   |
|                 15 | F   |
+--------------------+-----+
```

### 8. Using the GROUP BY operator, count the number of absences for each student. Display the student id and the count.

SQL:
```sql
SELECT student.student_id, COUNT(*) AS `number_of_absences` FROM student, absence WHERE student.student_id=absence.student_id GROUP BY student.student_id;
```

Output:
```
+------------+--------------------+
| student_id | number_of_absences |
+------------+--------------------+
|          5 |                  1 |
|         10 |                  2 |
|         17 |                  1 |
|         20 |                  1 |
+------------+--------------------+
```

### 9. Using NULL, find all the employees who have notes in their employee record.

SQL:
```sql
SELECT first_name, last_name, company, notes FROM employees WHERE notes IS NOT NULL;
```

Output:
```
+------------+----------------+-------------------+-------------------------------------------------------------------------------------------------------------------------+
| first_name | last_name      | company           | notes                                                                                                                   |
+------------+----------------+-------------------+-------------------------------------------------------------------------------------------------------------------------+
| Andrew     | Cencini        | Northwind Traders | Joined the company as a sales representative, was promoted to sales manager and was then named vice president of sales. |
| Jan        | Kotas          | Northwind Traders | Was hired as a sales associate and was promoted to sales representative.                                                |
| Steven     | Thorpe         | Northwind Traders | Joined the company as a sales representative and was promoted to sales manager.  Fluent in French.                      |
| Michael    | Neipper        | Northwind Traders | Fluent in Japanese and can read and write French, Portuguese, and Spanish.                                              |
| Laura      | Giussani       | Northwind Traders | Reads and writes French.                                                                                                |
| Anne       | Hellung-Larsen | Northwind Traders | Fluent in French and German.                                                                                            |
+------------+----------------+-------------------+-------------------------------------------------------------------------------------------------------------------------+
```

### 10. Using NULL, find all the orders in the order_details table that do not have a purchase order assigned to them.

SQL:
```sql
SELECT * FROM order_details WHERE purchase_order_id IS NULL;
```

Output:
```
+----+----------+------------+----------+------------+----------+-----------+----------------+-------------------+--------------+
| id | order_id | product_id | quantity | unit_price | discount | status_id | date_allocated | purchase_order_id | inventory_id |
+----+----------+------------+----------+------------+----------+-----------+----------------+-------------------+--------------+
| 28 |       30 |         80 |  30.0000 |     3.5000 |        0 |         2 | NULL           |              NULL |           63 |
| 29 |       31 |          7 |  10.0000 |    30.0000 |        0 |         2 | NULL           |              NULL |           64 |
| 30 |       31 |         51 |  10.0000 |    53.0000 |        0 |         2 | NULL           |              NULL |           65 |
| 31 |       31 |         80 |  10.0000 |     3.5000 |        0 |         2 | NULL           |              NULL |           66 |
| 32 |       32 |          1 |  15.0000 |    18.0000 |        0 |         2 | NULL           |              NULL |           67 |
| 33 |       32 |         43 |  20.0000 |    46.0000 |        0 |         2 | NULL           |              NULL |           68 |
| 35 |       34 |         19 |  20.0000 |     9.2000 |        0 |         2 | NULL           |              NULL |           69 |
| 36 |       35 |         48 |  10.0000 |    12.7500 |        0 |         2 | NULL           |              NULL |           70 |
| 38 |       37 |          8 |  17.0000 |    40.0000 |        0 |         2 | NULL           |              NULL |           71 |
| 43 |       42 |          6 |  10.0000 |    25.0000 |        0 |         2 | NULL           |              NULL |           84 |
| 44 |       42 |          4 |  10.0000 |    22.0000 |        0 |         2 | NULL           |              NULL |           85 |
| 46 |       43 |         80 |  20.0000 |     3.5000 |        0 |         1 | NULL           |              NULL |           86 |
| 47 |       43 |         81 |  50.0000 |     2.9900 |        0 |         1 | NULL           |              NULL |           87 |
| 48 |       44 |          1 |  25.0000 |    18.0000 |        0 |         1 | NULL           |              NULL |           88 |
| 49 |       44 |         43 |  25.0000 |    46.0000 |        0 |         1 | NULL           |              NULL |           89 |
| 50 |       44 |         81 |  25.0000 |     2.9900 |        0 |         1 | NULL           |              NULL |           90 |
| 52 |       45 |         40 |  50.0000 |    18.4000 |        0 |         2 | NULL           |              NULL |           91 |
| 59 |       50 |         21 |  20.0000 |    10.0000 |        0 |         2 | NULL           |              NULL |           92 |
| 60 |       51 |          5 |  25.0000 |    21.3500 |        0 |         2 | NULL           |              NULL |           93 |
| 61 |       51 |         41 |  30.0000 |     9.6500 |        0 |         2 | NULL           |              NULL |           94 |
| 62 |       51 |         40 |  30.0000 |    18.4000 |        0 |         2 | NULL           |              NULL |           95 |
| 67 |       55 |         34 |  87.0000 |    14.0000 |        0 |         2 | NULL           |              NULL |          117 |
| 68 |       79 |          7 |  30.0000 |    30.0000 |        0 |         2 | NULL           |              NULL |          119 |
| 69 |       79 |         51 |  30.0000 |    53.0000 |        0 |         2 | NULL           |              NULL |          118 |
| 70 |       78 |         17 |  40.0000 |    39.0000 |        0 |         2 | NULL           |              NULL |          120 |
| 71 |       77 |          6 |  90.0000 |    25.0000 |        0 |         2 | NULL           |              NULL |          121 |
| 72 |       76 |          4 |  30.0000 |    22.0000 |        0 |         2 | NULL           |              NULL |          122 |
| 73 |       75 |         48 |  40.0000 |    12.7500 |        0 |         2 | NULL           |              NULL |          123 |
| 74 |       74 |         48 |  40.0000 |    12.7500 |        0 |         2 | NULL           |              NULL |          124 |
| 75 |       73 |         41 |  10.0000 |     9.6500 |        0 |         2 | NULL           |              NULL |          125 |
| 76 |       72 |         43 |   5.0000 |    46.0000 |        0 |         2 | NULL           |              NULL |          126 |
| 77 |       71 |         40 |  40.0000 |    18.4000 |        0 |         2 | NULL           |              NULL |          127 |
| 78 |       70 |          8 |  20.0000 |    40.0000 |        0 |         2 | NULL           |              NULL |          128 |
| 79 |       69 |         80 |  15.0000 |     3.5000 |        0 |         2 | NULL           |              NULL |          129 |
| 80 |       67 |         74 |  20.0000 |    10.0000 |        0 |         2 | NULL           |              NULL |          130 |
| 81 |       60 |         72 |  40.0000 |    34.8000 |        0 |         2 | NULL           |              NULL |          131 |
| 82 |       63 |          3 |  50.0000 |    10.0000 |        0 |         2 | NULL           |              NULL |          132 |
| 83 |       63 |          8 |   3.0000 |    40.0000 |        0 |         2 | NULL           |              NULL |          133 |
| 84 |       58 |         20 |  40.0000 |    81.0000 |        0 |         2 | NULL           |              NULL |          134 |
| 85 |       58 |         52 |  40.0000 |     7.0000 |        0 |         2 | NULL           |              NULL |          135 |
| 86 |       80 |         56 |  10.0000 |    38.0000 |        0 |         1 | NULL           |              NULL |          136 |
| 90 |       81 |         81 |   0.0000 |     2.9900 |        0 |         5 | NULL           |              NULL |         NULL |
| 91 |       81 |         56 |   0.0000 |    38.0000 |        0 |         0 | NULL           |              NULL |         NULL |
+----+----------+------------+----------+------------+----------+-----------+----------------+-------------------+--------------+
```
