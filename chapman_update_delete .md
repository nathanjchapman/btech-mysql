# Update & Delete Assignment

For each of the questions in this assignment include the SQL query and the results of the query. Use the database and tables that you created in the database creation and insert data modules.

### 1. `There` was a mistake with the absence table. Peter, student id = 10, was not absent on September 9, 2012. It was Michael, student id = 11, that was absent. Update the table to correct this error.

SQL:
```sql
UPDATE absence SET student_id = 11 WHERE `date` = "2012-09-09";
```

Output:
```
Query OK, 0 rows affected (0.001 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```


### 2. There was a problem with the quiz on September 16, 2012 so we have decided to delete the scores and the grade events. Find out the id for the quiz and delete the scores from the score table and the event from the grade_event table.

SQL and Output:
```sql
DELETE FROM score WHERE event_id = 4;
-- Query OK, 27 rows affected (0.006 sec)
DELETE FROM grade_event WHERE event_id = 4;
-- Query OK, 1 row affected (0.006 sec)
```


### 3. Gender information for the student Avery is incorrect. Change the gender from 'F' to 'M' in the student table.

SQL:
```sql
UPDATE student SET sex = 'M' WHERE name = 'Avery';
```

Output:
```
Query OK, 1 row affected (0.006 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

###### 4. Change the student name 'Liesl' to 'Leslie' in the student table.

SQL:
```sql
UPDATE student SET name = 'Leslie' WHERE name = 'Liesl';
```

Output:
```
Query OK, 1 row affected (0.007 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

### 5. Remove student 'Devri' from the student table. Make note of the student id and remove all records from scores and absences for Devri.

SQL and Output:
```sql
SELECT student_id FROM student WHERE name = 'Devri'; -- 13
DELETE FROM score WHERE student_id = 13;
-- Query OK, 3 rows affected (0.004 sec)
DELETE FROM absence WHERE student_id = 13;
-- Query OK, 0 rows affected (0.000 sec)
-- The above is empty because the student had no absences
DELETE FROM student WHERE student_id = 13;
-- Query OK, 1 row affected (0.007 sec)
```

