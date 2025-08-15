# Database Study Roadmap

A structured guide to revise **Database & SQL concepts** — from fundamentals to advanced topics.

---

## **1. Database Fundamentals**

- **Difference between DB, DBMS, and RDBMS**
  - **Database (DB):** Organized collection of data.
  - **DBMS:** Software for managing databases (e.g., MySQL, MongoDB).
  - **RDBMS:** DBMS based on the relational model (e.g., MySQL, PostgreSQL).

- **Relational vs. Non-Relational Databases**
  - **Relational:** Structured tables, SQL (e.g., MySQL, PostgreSQL).
  - **Non-Relational:** Flexible schema, various data models (e.g., MongoDB, Redis).

- **Tables, Rows, Columns** – Basic structure of a relational database.
- **Primary Key** – Unique identifier for each record.
- **Foreign Key** – Field linking one table to another’s primary key.
- **Constraints** – Rules to maintain data integrity:
  - NOT NULL
  - UNIQUE
  - CHECK
  - DEFAULT

---

## **2. SQL Essentials**

- **DDL (Data Definition Language)** – `CREATE`, `ALTER`, `DROP`
- **DML (Data Manipulation Language)** – `INSERT`, `UPDATE`, `DELETE`, `SELECT`
- **Filtering & Sorting** – `WHERE`, `ORDER BY`, `DISTINCT`
- **Aggregations** – `GROUP BY`, `HAVING`, `COUNT`, `SUM`, `AVG`
- **JOINs** – Combining data from multiple tables:
  - **INNER JOIN** – Matching rows in both tables.
  - **LEFT JOIN** – All rows from left table + matches.
  - **RIGHT JOIN** – All rows from right table + matches.
  - **FULL OUTER JOIN** – All rows when there’s a match in either table.
  - **CROSS JOIN** – Cartesian product of both tables.
- **When to Use Each Join** – Based on data availability and matching needs.

---

## **3. Relationships & Data Modeling**

- **Relationship Types**:
  - One-to-One
  - One-to-Many
  - Many-to-Many
- **Normalization** – 1NF, 2NF, 3NF to reduce redundancy.
- **When to Denormalize** – For performance or reporting needs.
- **ER Diagrams (Entity-Relationship)** – Visual representation of entities and their relationships.

---

## **4. Indexing & Performance**

- **Indexes** – Clustered vs. Non-Clustered.
- **Performance Effects** – Improve SELECT speed, but can slow INSERT/UPDATE.
- **EXPLAIN / Execution Plans** – Understanding how queries run and optimizing them.

---

## **5. Transactions & ACID**

- **ACID Properties**:
  - **Atomicity** – All or nothing.
  - **Consistency** – Valid state transitions.
  - **Isolation** – Transactions don’t interfere.
  - **Durability** – Changes persist after commit.
- **Transaction Control Commands** – `BEGIN`, `COMMIT`, `ROLLBACK`.
- **Isolation Levels & Concurrency Issues**:
  - Dirty Read
  - Non-Repeatable Read
  - Phantom Read

---

## **6. Stored Routines & Views**

- **Stored Procedures** – Reusable SQL logic stored in the database.
- **Views** – Virtual tables based on queries.
- **Triggers** – Automatically executed logic in response to events.

---

## **7. Security & Backup**

- **User Roles & Permissions** – `GRANT`, `REVOKE`
- **SQL Injection Prevention** – Parameterized queries
- **Encryption** – At rest & in transit
- **Backups & Recovery Strategies** – Full, incremental, point-in-time recovery

---

## **8. Advanced Topics**

- **Partitioning** – Horizontal & vertical
- **Sharding** – Distributing data across servers
- **Replication** – Master-slave, master-master setups
- **Caching** – Using Redis/Memcached
- **NoSQL Concepts** – CAP theorem, eventual consistency

---
