---
description: Based on "SQL Tutorial - Full Database Course for Beginners"
---

# SQL Reference

```sql
/* Reference Commands for mySQL */

/* Data Types
 * INT
 * DECIMAL(M,N) -- total digits, precision
 * VARCHAR(1)   -- strings
 * BLOB         -- Binary Large Objects
 * DATE         -- 'YYYY-MM-DD'
 * TIMESTAMP    -- 'YYYY-MM-DD HH:MM:SS'
 */

/* Comparison Operators
 * <, >, <=, >=
 * <>, =
 * AND, OR
 */

/* 1. Show the existing databases */
SHOW DATABASES;


/* 2. Select an already existing database */
USE world;

/* 3. List the existing tables of the DB */
SHOW TABLES;

 /* 4. Create table 
  * On creation, we can add field requirements/constraints
  * NOT NULL specifies that no entry for that field can be null
  * UNIQUE specifies that each entry for that field needs to be unique.
  * PRIMARY KEY is basically a combo of not null and unique.
  * DEFAULT <value> specifies a value for null entries
  * AUTO_INCREMENT specifies that the data automatically is increased
 */
 CREATE TABLE student (
 	student_id INT PRIMARY KEY,
 	name VARCHAR(20),
 	major VARCHAR(20)
);

 /* 5. Show general info about a table.*/
 DESCRIBE student;

 /* 6. Remove a table.*/
 DROP student;

 /* 7. Add a column after creation.
  *    Remove a column after creation. */
 ALTER TABLE student ADD gpa DECIMAL(3,2);
 ALTER TABLE student DROP gpa;
 

 /* 8. Insert values (standard).
       Insert values to specific columns. */
INSERT INTO student VALUES (1, 'Jack', 'Biology');
INSERT INTO student VALUES (2, 'Kate', 'Sociology');
INSERT INTO student(student_id, major) VALUES (3, 'Chemistry'); # We don't know student's name

-- 9. Show the full table
SELECT * FROM student;

-- 10. Delete all records from table
DELETE FROM student;

-- 11. Update Records by value replacement
UPDATE student
SET major = 'BioChemistry'
WHERE major = 'Biology' OR major = 'Chemistry';

-- similar with IN subquery
UPDATE student
SET major = 'BioChemistry'
WHERE major IN ('Biology', 'Chemistry');


-- 12. Update Records by changing all rows
UPDATE student
SET major = 'undecided';

-- 13. Delete specific records
DELETE FROM student
WHERE student_id = 5;

-- 14. Hall of Fame: Get the first N records
SELECT name, major
FROM student
LIMIT 5;


/* Tutorial - Load From CSV
 * 1. Check the secure private folder of mysql with
 * SHOW VARIABLES LIKE "secure_file_priv";
 * 2. Check if local infile is enabled
 * SHOW VARIABLES LIKE "local_infile";
 * 2.a If not, set it
 * SET GLOBAL local_infile = 1;
 * 3. Copy file to secure folder and load from there
 *	
 * 	load data infile '/var/lib/mysql-files/students_data.csv'
 *	into table student 
 *	fields terminated by ',' 
 *	enclosed by '"'
 *	lines terminated by '\n';
 *
 * 4. If header, add IGNORE 1 ROWS
*/
```

