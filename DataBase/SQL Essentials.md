![Full_Join](https://github.com/user-attachments/assets/2a23a08f-e9f7-4a90-a606-52767accf712)SQL Commands are mainly categorized into five categories: 

- DDL – Data Definition Language
- DQL – Data Query Language
- DML – Data Manipulation Language
- DCL – Data Control Language
- TCL - Transaction Control Language


<img width="797" height="669" alt="new" src="https://github.com/user-attachments/assets/0f5a1d39-85ab-4a5a-94cf-7df1800eabac" />


1. DDL - Data Definition Language
altering, creating, and deleting database structure, like tables and indexes

create , alter , drop , truncate 


2. DQL - Data Query Language
DQL statements are used for performing queries on the data within schema objects.

- note :
When a SELECT is fired against a table or tables the result is compiled into a further temporary table, which is displayed or perhaps received by the program.

select, where, from, order by, and limit 


3. DML - Data Manipulation Language
handles communication with data already existing in our DB tables
controls access to the data that exists in DB

insert, update, delete 


4. DCL - Data Control Language
deals with rights and permissions and other controls

 grant and revoke 

- grant: is used to assign a permission to a user specifying the operations the user has access to 
- revoke : used to remove the assigned access or privilege from this user and throw away its roles  

 example : 

 ```sql
GRANT SELECT, UPDATE ON employees TO Reyad;
```
this command grants reyad the permission to select and update records in employee table only 



5. TCL - Transaction Control Language
First what is transition :
transaction is a set of tasks that are done together and grouped in a single execution unit , if one task of these tasks failed the full transaction will fail (all or no one)

begin transaction, commit , rollback and save point 

Example to clarify :

BEGIN TRANSACTION;
UPDATE employees SET department = 'Marketing' WHERE department = 'Sales';
SAVEPOINT before_update;
UPDATE employees SET department = 'IT' WHERE department = 'HR';
ROLLBACK TO SAVEPOINT before_update;
COMMIT;
In this example, we have saved a state with a savePoint so the transaction can roll back and back to the savepoint before being committed

## Joins 

SQL joins are fundamental tools for combining data from multiple tables in relational databases.

###Types of SQL Joins 

1. Inner join
    
(the condition is satisfied ) is the main rule 
<img width="502" height="329" alt="SQL-Join" src="https://github.com/user-attachments/assets/e75b170f-36ec-4274-8294-5cd4bb413512" />

selects and combining all rows from talbes where the condiion is satisfied

here is an example 
imagine you have two tables one for student another for student course and you try to get all info from joining two tables with each other 

so you join based on you references exist in tables (for example here ROLL_NO is in student_course is a reference to TOLL_NO primary key in student table)

```sql 
SELECT StudentCourse.COURSE_ID, Student.NAME, Student.AGE FROM Student
INNER JOIN StudentCourse
ON Student.ROLL_NO = StudentCourse.ROLL_NO;
```



2. SQL LEFT JOIN
returns all rows from left table with matching rows , if not exist matching rows then rows will be retrived with null values returned for columns of right table

<img width="502" height="329" alt="Left_Join" src="https://github.com/user-attachments/assets/e9c133b1-a596-4d20-85b9-194322b29c76" />

here is an example to clarify 

```sql
SELECT Student.NAME,StudentCourse.COURSE_ID 
FROM Student
LEFT JOIN StudentCourse 
ON StudentCourse.ROLL_NO = Student.ROLL_NO;
```


![table31](https://github.com/user-attachments/assets/6cc91cf2-6999-42cc-9452-b43dec447fb2)


you can notice here some rows contain null values 
as there is no matching happens here in these row so you get only data exist in left table 


3. SQL RIGHT JOIN
it is like left join return all matching rows fron two tables and it returns all the right side table data although there is no mathicng with it in left table ....

![Right_join](https://github.com/user-attachments/assets/84eecaff-160b-405a-9949-82ab1fe51cfe)


sql example 

```sql
SELECT Student.NAME,StudentCourse.COURSE_ID 
FROM Student
RIGHT JOIN StudentCourse 
ON StudentCourse.ROLL_NO = Student.ROLL_NO;
```

here the full right table will be retrived with rows that has null values if no there matching 



4. SQL FULL JOIN
the result will contain all the rows from both tables with null values for no matching rows (combines the result of left join and right join)

![Full_Join](https://github.com/user-attachments/assets/fe100652-ce48-4208-8900-470ec0b16a5b)


```sql
SELECT Student.NAME,StudentCourse.COURSE_ID 
FROM Student
FULL JOIN StudentCourse 
ON StudentCourse.ROLL_NO = Student.ROLL_NO;
```



5. Natural Join
A Natural Join is a type of INNER JOIN that automatically joins two tables based on columns with the same name and data type.
It returns only the rows where the values in the common columns match.


```sql
SELECT student_id, name, dept_name
FROM Students
NATURAL JOIN Departments;
```

NATURAL JOIN automatically detects that both tables have a column called dept_id.
It joins them on Students.dept_id = Departments.dept_id.



6. cross Join

SQL CROSS JOIN is a powerful tool for generating all possible combinations of rows from two or more tables ( till now i do not know why it is exist :) )

a use case we can imagine to use cross join 


-Simulation or Probability Scenarios
  Example: Rolling two dice.
  Dice1 (values 1–6)
  Dice2 (values 1–6)
  CROSS JOIN → All 36 outcomes → can calculate probabilities.

so it is used whenever we want all possible combinations of two data sets 


example : 

```sql
SELECT *
FROM CUSTOMER
CROSS JOIN ORDERS;
```
and here is the reuslt of this query

<img width="660" height="282" alt="Screenshot-2023-08-10-200309-660" src="https://github.com/user-attachments/assets/9c6ae207-55f6-44c5-bfd2-2ada3260a92d" />

