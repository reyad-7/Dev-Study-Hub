# Normalization

- here we try to have a good relations between tables so if we want to add we will not add any redundant data , if we want to update anything we do not want to update a huge number of records based on these change.

- notice here all operations are coupled with each others (insert , delete and update )

- so we try to normalize our schema or our talbes structure to keep it simple and scalable 


These are the problems we have 

so we will continue exploring normalization to conquer this problem 


so, what is Normalization: the process of decomposing big fat tables and trying to break them down to have tables with smaller relations 

so how can we divide tables ?? 
Based on what? 

## First Normal Form (1St NF) : 
conquer 
- composite attributes
- Multivalued attributes
- nested relations

Now what is our work in this from ?
first we try to get and know which attributes cause a redundancy in our data rows (redundant here means repeating values in rows)
so our work is to get these attributes that cause redundancy and try to put in separate tables and reference to them with the main table primary key ...


so our steps : 
you search for a violation then you try to fix it 

example :

| ContactID | Name    | PhoneNumbers                | Street          | City  | Country |
| --------- | ------- | --------------------------- | --------------- | ----- | ------- |
| 1         | Ahmed   | `010-111, 012-222`          | 12 Nile St      | Cairo | Egypt   |
| 2         | Mariam  | `015-333`                   | 5 Tahrir Sq     | Cairo | Egypt   |
| 3         | Youssef | `010-444, 011-555, 012-666` | 89 Pyramids Ave | Giza  | Egypt   |


What’s wrong here?
PhoneNumbers stores multiple values in one cell (not atomic) → violates 1NF.
If you try to “fix” it by adding Phone1, Phone2, Phone3, that becomes a repeating group → also violates 1NF.


Converting to 1NF (good design)
Split multi-valued attributes into a separate table

```sql 
CREATE TABLE Contacts (
  ContactID   INT PRIMARY KEY,
  Name        VARCHAR(100),
  Street      VARCHAR(200),
  City        VARCHAR(100),
  Country     VARCHAR(100)
);

```

```sql 
CREATE TABLE ContactPhones (
  PhoneID     INT PRIMARY KEY,
  ContactID   INT NOT NULL,
  PhoneNumber VARCHAR(30) NOT NULL,
  PhoneType   VARCHAR(20), -- e.g., Mobile, Home, Work
  CONSTRAINT FK_ContactPhones_Contacts
    FOREIGN KEY (ContactID) REFERENCES Contacts(ContactID)
);

```
here are some sample data to jsut practice 

```sql
INSERT INTO Contacts (ContactID, Name, Street, City, Country) VALUES
(1, 'Ahmed', '12 Nile St', 'Cairo', 'Egypt'),
(2, 'Mariam', '5 Tahrir Sq', 'Cairo', 'Egypt'),
(3, 'Youssef', '89 Pyramids Ave', 'Giza', 'Egypt');

INSERT INTO ContactPhones (PhoneID, ContactID, PhoneNumber, PhoneType) VALUES
(101, 1, '010-111', 'Mobile'),
(102, 1, '012-222', 'Home'),
(103, 2, '015-333', 'Mobile'),
(104, 3, '010-444', 'Mobile'),
(105, 3, '011-555', 'Work'),
(106, 3, '012-666', 'Home');

```

if you want to get All phones for a contact:

```sql
SELECT c.Name, p.PhoneNumber, p.PhoneType
FROM Contacts c
JOIN ContactPhones p ON p.ContactID = c.ContactID
WHERE c.Name = 'Youssef';
```


so as you see in the example above, you isolated the column with MV attributes into a separate table to conquer redundancy and prevent repeated columns in data rows 




## second normal form (2ND NF)

### you should be in 1St NF 

- you try to conquer partial dependency between an attribute and the primary key 

-- if you have a composite primary key 
-- so you should isolate and make all attributes in one table depends on the fully primary key in this table 
-- so if you have an attribute that depend on only a part of the primary key so you separate this attribte with this only part of this primary key in a separate table and reference to it with the primary key 



example to showcase and understand 

here is our data 

| StudentID | CourseID | StudentName | Major | CourseTitle | Credits | Grade |
| --------- | -------- | ----------- | ----- | ----------- | ------- | ----- |
| 1001      | CS101    | Ahmed Ali   | CS    | Databases   | 3       | A-    |
| 1001      | CS102    | Ahmed Ali   | CS    | Networks    | 3       | B+    |
| 1002      | CS101    | Sara Hassan | IS    | Databases   | 3       | A     |


What’s wrong?

- StudentName, Major depend only on StudentID (a part of the key).
- CourseTitle, Credits depend only on CourseID (the other part of the key).
- Only Grade depends on the whole key (StudentID, CourseID).


This creates anomalies:

- Update anomaly: Change “Databases” title? You must update many rows.
- Insert anomaly: You can’t insert a new course until a student enrolls.
- Delete anomaly: Deleting the last enrollment in CS101 loses the course info.



now we will restructure our db schema again 


```sql

-- Students: facts about the student (depend on StudentID)
CREATE TABLE Students (
  StudentID   INT PRIMARY KEY,
  StudentName VARCHAR(100) NOT NULL,
  Major       VARCHAR(50)  NOT NULL
);

-- Courses: facts about the course (depend on CourseID)
CREATE TABLE Courses (
  CourseID    VARCHAR(20) PRIMARY KEY,
  CourseTitle VARCHAR(100) NOT NULL,
  Credits     INT NOT NULL CHECK (Credits > 0)
);

-- Enrollments: relationship facts (depend on the whole (StudentID, CourseID))
CREATE TABLE Enrollments (
  StudentID INT         NOT NULL,
  CourseID  VARCHAR(20) NOT NULL,
  Grade     VARCHAR(5),
  PRIMARY KEY (StudentID, CourseID),
  FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
  FOREIGN KEY (CourseID)  REFERENCES Courses(CourseID)
);

```
and here a query to get all info you want 

```sql
SELECT s.StudentID, s.StudentName, s.Major,
       c.CourseID, c.CourseTitle, c.Credits,
       e.Grade
FROM Enrollments e
JOIN Students   s ON s.StudentID = e.StudentID
JOIN Courses    c ON c.CourseID  = e.CourseID;

```
now :
- all students relevant data are stored in student table 
- course relevent data are stored in courses table 
- enrollments data are stored in table contain full key that major depend on it 







## third normal from (3RD NF)

you should be in 2ND NF 

You try to conquer 
- transitive dependency 

what is transitive dependency? 

Non-key attribute depends on another non-key attribute we will see an example to show and explain more 
But for simplicity, an attribute depends on and is formed by another attribute, and this attribute is not a primary key of this table 


here is another example for definition 

A table is in Third Normal Form if:

- It is already in 2NF (no partial dependencies on a composite key).

- No non-key column depends on another non-key column (i.e., no transitive dependencies).

### Quick mantra: Every non-key attribute depends on “the key, the whole key, and nothing but the key.”



so let us have an example to explain 
first we have this table with violations 


| EmpID | EmpName | DeptID | DeptName    | DeptLocation | ManagerID | ManagerName |
| ----: | ------- | ------ | ----------- | ------------ | --------- | ----------- |
|     1 | Ahmed   | D10    | Accounting  | Cairo        | M1        | Mona        |
|     2 | Salma   | D10    | Accounting  | Cairo        | M1        | Mona        |
|     3 | Youssef | D20    | Engineering | Giza         | M2        | Kareem      |


here is the Functional dependencies (Normal dependencies):

EmpID → EmpName, DeptID

DeptID → DeptName, DeptLocation, ManagerID

ManagerID → ManagerName



Anomalies:

Update: Renaming “Accounting” requires updating many rows.

Insert: Can’t add a new Department until an Employee exists.

Delete: Deleting the last employee in D10 loses the department & manager info.


but if you noticed DeptName, DeptLocation, ManagerID and ManagerName dont fully depend on primary key (EmpID) they depend on other attributes (non-key attributes)
so there is a transitive dpendncies here with these attributes 
so what we need to do ??
first we will isolate these attributes with their key that they depend on in a separate tables 

so we will have **3** tables 


```sql
-- Managers: facts about managers
CREATE TABLE Managers (
  ManagerID   VARCHAR(10) PRIMARY KEY,
  ManagerName VARCHAR(100) NOT NULL
);

-- Departments: facts about departments (each has a manager)
CREATE TABLE Departments (
  DeptID       VARCHAR(10) PRIMARY KEY,
  DeptName     VARCHAR(100) NOT NULL,
  DeptLocation VARCHAR(100) NOT NULL,
  ManagerID    VARCHAR(10)  NOT NULL,
  FOREIGN KEY (ManagerID) REFERENCES Managers(ManagerID)
);

-- Employees: facts about employees
CREATE TABLE Employees (
  EmpID   INT PRIMARY KEY,
  EmpName VARCHAR(100) NOT NULL,
  DeptID  VARCHAR(10)  NOT NULL,
  FOREIGN KEY (DeptID) REFERENCES Departments(DeptID)
);

```

and here is a query to show all data view that was before 

```sql
SELECT e.EmpID, e.EmpName,
       d.DeptID, d.DeptName, d.DeptLocation,
       m.ManagerID, m.ManagerName
FROM Employees e
JOIN Departments d ON d.DeptID = e.DeptID
JOIN Managers    m ON m.ManagerID = d.ManagerID;

``
