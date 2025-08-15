# Database Fundamentals


##Difference Between RDBMS and DBMS

### DBMS 

A Database Management System (DBMS) is a software that is used to define, create, and maintain a database and provides controlled access to the data.

It is a management system that is used to manage the entire flow of data, i.e, the insertion of data or the retrieval of data, how the data is inserted into the database,
or how fast the data should be retrieved, so DBMS takes care of all these features, as it maintains the uniformity of the database as well does the faster insertions as well as retrievals


### RDBMS: 
It is a type of DBMS; as the name suggests, it deals with relations as well as various key constraints.
So here we have tables, which are called schemas, and we have rows, which are called tuples. It also aids in the reduction of data redundancy and the preservation of database integrity




### relational database: 
--A relational database stores data in tables composed of rows and columns. 
In a relational database, data is contained within a table, which is then linked to data contained within other tables through the use of unique identifying keys. Specifically, relationships between tables are formed when a primary key, which uniquely identifies a row in one table, connects with a foreign key, identifying a row of data in another table. 

-- it is designed to store structured data or well-defined data in tables (rows and columns form )
-- SQL is the most common programming language used to interface with relational databases within relational database management systems (RDMS)

--Basic structure of relational db is tables , rows and columns 
-- MySQL --
-- PostgreSQL --
-- Microsoft SQL Server --

### non-relational database
A non-relational database is a type of database that doesn’t store data in tables, but instead, stores it in whatever format is best for the type of data being stored.
In effect, non-relational databases are designed to contain *unstructured data*, or loosely defined data like email messages, videos, images,and business documents that aren’t easily standardized.
They can also be used to store a mix of structured and unstructured data. 
Non-relational databases are said to be NoSQL, meaning that they don’t use structured query language, even though many NoSQL databases do support SQL queries.

There are four types of non-relational databases 
- Key-value stores: In a key-value store, data is assigned a unique identifier, which allows it to be retrieved and sorted.
- Column-family data stores: In a column-family data store, data is organized in a "keyspace" containing multiple families of different columns.
- Graph databases: Graph databases store data in nodes and structure them based on their relationships to one another
- Document databases: Document databases store data within documents, which typically contain one object and all its associated metadata.

examples:
-- Apache Cassandra
-- MongoDB



### SQL PRIMARY KEY Constraint

The **PRIMARY KEY** constraint is used to uniquely identify each record in a table.
Primary keys must contain unique values and cannot contain NULL values.
Each table can have only ONE primary key. The primary key can be a single column or a combination of columns (Composite).


### FOREIGN KEY
The FOREIGN KEY constraint is a key used to link two tables together.
A FOREIGN KEY is a field or collection of fields in a table that refers to the PRIMARY KEY in another table.

Example:  a personId foreign key in orders table
every order should have a person so we reference this order to a person by a foreign key (person id )

```sql
CREATE TABLE Orders (
    OrderID int NOT NULL,
    OrderNumber int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)   // here is the foreign key that references a person 
);
```




### Constraints – Rules to maintain data integrity 

The following constraints are commonly used in SQL:

-- NOT NULL - Ensures that a column cannot have a NULL value
  This enforces a field to always contain a value, which means that you cannot insert a new record, or update a record without adding a value to this field.
-- UNIQUE - Ensures that all values in a column are different
  You can define Unique constraints on multiple columns as shown below 
  
```sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CONSTRAINT UC_Person UNIQUE (ID,LastName)
);
```

-- PRIMARY KEY - A combination of a NOT NULL and UNIQUE. Uniquely identifies each row in a table

-- FOREIGN KEY - Prevents actions that would destroy links between tables
  it is used to prevent actions that will destroy links between tables like deleting that person that already has an order 
  so how can i handle the case of deletion this reference (person id ) in order  table also you should handle the case of inserting invalid data into the foreign key columns as it validates insertion
  Values should exist in our data  

-- CHECK - Ensures that the values in a column satisfy a specific condition
  it like a condition should be satisfied to be able to insert to this column 
  see example below 
  ``` sql
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CHECK (Age>=18)
);
```
here is the age value should be greater than or equal 18 

-- DEFAULT - Sets a default value for a column if no value is specified
  It is used to insert the specified default value instead of null ***if you did not insert any other value***  

  
