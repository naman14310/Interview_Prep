# SQL

<br>

## SQL Vs MySQL

#### SQL
1. Structured Query Language a.k.a., SQL is the standard language used to operate, manage, and access databases. 
2. The American National Standards Institute (ANSI) maintains that SQL is the standard language for managing a relational database management system. It is owned, hosted, maintained, and offered by Microsoft. 

<br>

#### MySQL
1. MySQL is an open-source relational database management system that uses SQL commands to perform specific functions/operations in a database.
2. It is owned and offered by Oracle Corporation. 
3. MySQL is written in the C and C++ programming languages.

<br>

## DDL | DQL | DML | DCL | TCL Commands

<br>

![img](https://media.geeksforgeeks.org/wp-content/uploads/20210920153429/new.png)

<br>

### DDL
DDL or **Data Definition Language** actually consists of the SQL commands that can be used to define the database schema.

### DQL
DQL or **Data Query Language** statements are used for performing queries on the data within schema objects.

### DML
DML or **Data Manipulation Language** commands deals with the manipulation of data present in the database. It is the component of the SQL statement that controls access to data and to the database.

### DCL
DCL or **Data Control Language** includes commands such as GRANT and REVOKE which mainly deal with the rights, permissions, and other controls of the database system. 

### TCL
TCL or **Transaction Control Language** deals with transactional statements.

<br>

## [Delete Vs Truncate](https://www.geeksforgeeks.org/difference-between-delete-and-truncate/)
DELETE is a DML(Data Manipulation Language) command and is used to remove or delete only those rows(tuple) that satisfy the where condition if specified otherwise by default it removes all the tuples(rows) from the table. 

TRUNCATE is a DDL(Data Definition Language) command and is used to delete all the rows or tuples from a table. Unlike the DELETE command, TRUNCATE command does not contain a WHERE clause. Unlike the DELETE command, the TRUNCATE command is fast. We cannot rollback the data after using the TRUNCATE command. 

<br>

## [SQL Views](https://www.geeksforgeeks.org/sql-views/)
Views in SQL are kind of virtual tables. We can create a view by selecting fields from one or more tables present in the database. A View can either have all the rows of a table or specific rows based on certain condition.

```
CREATE VIEW view_name AS
SELECT column1, column2.....
FROM table_name
WHERE condition;
```

<br>

## [SQL Trigger](https://www.geeksforgeeks.org/sql-trigger-student-database/)
A trigger is a stored procedure in database which automatically invokes whenever a special event in the database occurs. For example, a trigger can be invoked when a row is inserted into a specified table or when certain table columns are being updated.

```
create trigger [trigger_name] 
[before | after]  
{insert | update | delete}  
on [table_name]  
[for each row]  
[trigger_body] 
```

<br>

## [What is Cursor in SQL ?](https://www.geeksforgeeks.org/what-is-cursor-in-sql/)
Cursor is a Temporary Memory which is Allocated by Database Server at the Time of Performing DQL and DML(Data Manipulation Language) operations on Table by User. Cursors are used to store Database Tables. 

There are 2 types of Cursors: 

#### Implicit Cursors
Implicit Cursors are also known as Default Cursors of SQL SERVER. These Cursors are allocated by SQL SERVER when the user performs DML operations.

#### Explicit Cursors :
Explicit Cursors are Created by Users whenever the user requires them. Explicit Cursors are used for Fetching data from Table in Row-By-Row Manner.

<br>

## [SQL Injection](https://www.w3schools.com/sql/sql_injection.asp)
SQL injection is the placement of malicious code in SQL statements, via web page input. SQL injection usually occurs when you ask a user for input, like their username/userid, and instead of a name/id, the user gives you an SQL statement that you will unknowingly run on your database.

<br>



## Some Famous InterView Queries

#### 1. Find the employee record with the highest salary
```
Select * from employee where salary =(select max(salary) from employee);
```

<br>

#### 2. Find 3rd hightest salary
```
select * from employee order by salary desc limit 2,1;
```

<br>

#### 3. Find Nth highest salary without Top/limit keyword
```
select salary from employee e1 where n-1 = (select count(distinct salary) from employee e2 where e2.salary > e1.salary);
```

<br>

#### 4. Find duplicate rows in table
```
select *, count(empId) from employee group by empId having count(empId)>1;
```

<br>

#### 5. Display first and last record from employee table
```
select * from employee where empId = (select min(empId) from employee);

select * from employee where empId = (select max(empId) from employee);
```

<br>

#### 6. Retrieve last 3 records from employee table
```
select * from (select * from employee order by empId desc limit 3) temp order by empId asc;
```

<br>

#### 7. Delete rows having duplicate email IDs
```
For deleting duplicate records (leaving one row):
delete e1 from employee e1, employee e2 where e1.email = e2.email and e1.id > e2.id;

For deleting all duplicate records:
delete e1 from employee e1, employee e2 where e1.email = e2.email and e1.id != e2.id;
```

<br>

#### 8. Find number of employees whose DOB is between date1 to date2 and are grouped according to gender
```
select count(*), gender from employee where DOB between date1 and date2 group by gender;
```

<br>

#### 9. Fetch 50% records from table
```
SET @count = (select count(id)/2 from employee);
Prepare stmt from 'select * from employee limit ?';
Execute stmt using @count;
```

<br>

#### 10. Dislay total salary of each employee after adding 10% increment in the salary
```
select name, salary+(salary/10) as totalSalary from employee
```

<br>

#### 11. Find the Name of a Person Whose Name Starts with Specific Letter
```
SELECT * FROM department WHERE NAME LIKE 'A%';
```

<br>

#### 12. Create an empty table from an existing table in SQL?
```
CREATE TABLE new_table AS (SELECT * FROM old_table WHERE 1=2); 
```

<br>

#### 13. Fetch common records from two tables
```
(Select * from table1) Intersect (Select * from table2)  
```

<br>

#### 14. Fetch alternate records from a table in mysql
```
To fetch even Numbered row:
SELECT * FROM table_name WHERE column_name % 2 = 0

To fetch odd Numbered row:
SELECT * FROM table_name WHERE column_name % 2 = 1
```

<br>

#### 15. Select first 5 chars of a string
```
Extracting 5 characters from a string, starting in position 1:
SELECT SUBSTRING('SQL Tutorial', 1, 3) AS ExtractString;
```

Note: In SQL, Indexing will start from 1
