SQL Commands are mainly categorized into five categories: 

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
first what is transition :
transction is a set of tasks that are done together and grouped in a single execution unit , if one task of these tasks failed the full transaction will fail (all or no one)

begin tansacion , commit , rollback and save point 

example to clarify :

BEGIN TRANSACTION;
UPDATE employees SET department = 'Marketing' WHERE department = 'Sales';
SAVEPOINT before_update;
UPDATE employees SET department = 'IT' WHERE department = 'HR';
ROLLBACK TO SAVEPOINT before_update;
COMMIT;
in this example we have saved a state with a savePoint so the transaction can roll back and back to the savepoint before being committed

