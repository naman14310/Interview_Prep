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
