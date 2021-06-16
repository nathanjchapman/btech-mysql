# Advanced Insert Assignment

For each of the questions in this assignment include the SQL query and the results of the query. Use the following file for all exercises in this assignment: advance_insert.txt

### 1. Rewrite each of the insert queries in the advance_insert.txt file to be insert ignore queries.

SQL:
```sql
INSERT IGNORE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Megan','F',1);
INSERT IGNORE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Joseph','M',2);
INSERT IGNORE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Kyle','M',3);
INSERT IGNORE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Katie','F',4);
INSERT IGNORE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Abby','F',5);
INSERT IGNORE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Nathan','M',6);
INSERT IGNORE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Liesl','F',7);
INSERT IGNORE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Ian','M',8);
INSERT IGNORE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Colin','M',9);
INSERT IGNORE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Peter','M',10);
```

### 2. Rewrite each of the insert queries in the advance_insert.txt file to be insert update queries.

SQL:
```sql
INSERT INTO `student` (`name`, `sex`, `student_id`) VALUES ('Megan','F',1)
    ON DUPLICATE KEY UPDATE
    student_id = VALUES(student_id);
INSERT INTO `student` (`name`, `sex`, `student_id`) VALUES ('Joseph','M',2)
    ON DUPLICATE KEY UPDATE
    student_id = VALUES(student_id);
INSERT INTO `student` (`name`, `sex`, `student_id`) VALUES ('Kyle','M',3)
    ON DUPLICATE KEY UPDATE
    student_id = VALUES(student_id);
INSERT INTO `student` (`name`, `sex`, `student_id`) VALUES ('Katie','F',4)
    ON DUPLICATE KEY UPDATE
    student_id = VALUES(student_id);
INSERT INTO `student` (`name`, `sex`, `student_id`) VALUES ('Abby','F',5)
    ON DUPLICATE KEY UPDATE
    student_id = VALUES(student_id);
INSERT INTO `student` (`name`, `sex`, `student_id`) VALUES ('Nathan','M',6)
    ON DUPLICATE KEY UPDATE
    student_id = VALUES(student_id);
INSERT INTO `student` (`name`, `sex`, `student_id`) VALUES ('Liesl','F',7)
    ON DUPLICATE KEY UPDATE
    student_id = VALUES(student_id);
INSERT INTO `student` (`name`, `sex`, `student_id`) VALUES ('Ian','M',8)
    ON DUPLICATE KEY UPDATE
    student_id = VALUES(student_id);
INSERT INTO `student` (`name`, `sex`, `student_id`) VALUES ('Colin','M',9)
    ON DUPLICATE KEY UPDATE
    student_id = VALUES(student_id);
INSERT INTO `student` (`name`, `sex`, `student_id`) VALUES ('Peter','M',10)
    ON DUPLICATE KEY UPDATE
    student_id = VALUES(student_id);
```


### 3. Rewrite each of the insert queries in the advance_insert.txt file to be replace queries.

SQL:
```sql
REPLACE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Megan','F',1);
REPLACE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Joseph','M',2);
REPLACE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Kyle','M',3);
REPLACE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Katie','F',4);
REPLACE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Abby','F',5);
REPLACE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Nathan','M',6);
REPLACE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Liesl','F',7);
REPLACE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Ian','M',8);
REPLACE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Colin','M',9);
REPLACE INTO `student` (`name`, `sex`, `student_id`) VALUES ('Peter','M',10);
```
