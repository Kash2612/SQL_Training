# SQL_Training

## 1. Introduction to Databases
1. Types of databases: Relational vs. Non-relational
Relational databases store data in rows and columns like a spreadsheet while non-relational databases don't, using a storage model that is best suited for the type of data it’s storing.

2. SQL vs. NoSQL databases
SQL databases are primarily called Relational Databases (RDBMS); whereas NoSQL databases are primarily called non-relational or distributed databases. <br/>

- A product table in SQL Database accept data looking like this:
``` sql
{
"id": "101",
"category":"food"
"name":"Apples",
"qty":"150"
}
```

- NoSQL Database might save the products in many variations withouyt constraints
``` nosql
  Products=[
{
"id":"101:
"category":"food",,
"name":"California Apples",
"qty":"150"
},
{
"id":"102,
"category":"electronics"
"name":"Apple MacBook Air",
"qty":"10",
"specifications":{
   "storage":"256GB SSD",
   "cpu":"8 Core",
   "camera": "1080p FaceTime HD camera"
  }
}
]
```
## 2. Relational Database Fundamentals
### Entity-Relationship (ER) model
An ER diagram is a database model that illustrates entities (such as objects, people, or concepts) with their attributes (such as name, type, or age). Entities can be linked with one another; the nature of these links, or relationships, depends on how entities interact.

* Let's create and ER diagram that presents a real-world scenario where a doctor of a particular specialization prescribes certain medicines to a patient.

The Doctor entity has the following attributes: id, name and specialization.
The Medicines entity has the following attributes: id, name, type, and expiry date.
The Patients entity has the following attributes: id, ssn and name.

The relationships between the entities indicate that a Doctor prescribes Medicines to Patients.
![ER DIAGRAM](https://github.com/Kash2612/SQL_Training/blob/main/Untitled%20Diagram.png)

``` sql
CREATE DATABASE Hospital;
```
``` sql
USE Hospital;
```
``` sql
CREATE TABLE Doctor (
    DoctorID INT PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(100) NOT NULL,
    Specialization VARCHAR(100) NOT NULL
);

CREATE TABLE Medicines (
    MedicineID INT PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(100) NOT NULL,
    Type VARCHAR(50) NOT NULL,
    Expiry DATE NOT NULL
);

CREATE TABLE Patients (
    PatientID INT PRIMARY KEY AUTO_INCREMENT,
    SSN VARCHAR(11) NOT NULL UNIQUE,
    Name VARCHAR(100) NOT NULL
);

CREATE TABLE Prescriptions (
    PrescriptionID INT PRIMARY KEY AUTO_INCREMENT,
    DoctorID INT,
    PatientID INT,
    MedicineID INT,
    FOREIGN KEY (DoctorID) REFERENCES Doctor(DoctorID),
    FOREIGN KEY (PatientID) REFERENCES Patients(PatientID),
    FOREIGN KEY (MedicineID) REFERENCES Medicines(MedicineID)
);
```

``` sql
INSERT INTO Doctor (Name, Specialization) VALUES
('Dr. John Smith', 'Cardiology'),
('Dr. Jane Doe', 'Pediatrics');

INSERT INTO Medicines (Name, Type, Expiry) VALUES
('Aspirin', 'Painkiller', '2025-12-31'),
('Amoxicillin', 'Antibiotic', '2024-06-15'),
('Ibuprofen', 'Anti-inflammatory', '2026-03-10'),
('Metformin', 'Diabetes', '2024-09-30'),
('Lisinopril', 'Antihypertensive', '2025-05-20');


INSERT INTO Patients (SSN, Name) VALUES
('123-45-6789', 'Alice Johnson'),
('987-65-4321', 'Bob Brown');

INSERT INTO Prescriptions (DoctorID, PatientID, MedicineID) VALUES
(1, 1, 1),  -- Dr. John Smith prescribes Aspirin to Alice Johnson
(2, 2, 2);  -- Dr. Jane Doe prescribes Amoxicillin to Bob Brown
```

### Let's cross check the ER Diagram using SQL Queries

``` sql
SELECT * FROM Prescriptions;
```
![image](https://github.com/user-attachments/assets/7a5169c9-389a-409b-90f8-9d4cdd64765f)

``` sql
SELECT 
    p.PrescriptionID,
    d.Name AS DoctorName,
    d.Specialization,
    pat.Name AS PatientName,
    pat.SSN,
    m.Name AS MedicineName,
    m.Type,
    m.Expiry
FROM 
    Prescriptions p
JOIN 
    Doctor d ON p.DoctorID = d.DoctorID
JOIN 
    Patients pat ON p.PatientID = pat.PatientID
JOIN 
    Medicines m ON p.MedicineID = m.MedicineID;
```
![image](https://github.com/user-attachments/assets/015b55c5-162b-4a68-98a5-58e7e4077cf7)

``` sql
SELECT 
    d.Name AS DoctorName,
    pat.Name AS PatientName,
    m.Name AS MedicineName
FROM 
    Prescriptions p
JOIN 
    Doctor d ON p.DoctorID = d.DoctorID
JOIN 
    Patients pat ON p.PatientID = pat.PatientID
JOIN 
    Medicines m ON p.MedicineID = m.MedicineID
WHERE 
    d.Name = 'Dr. John Smith' AND pat.Name = 'Alice Johnson';
```
![image](https://github.com/user-attachments/assets/34e73532-de49-409a-9237-ed528b0c6ba5)







## 3. Basic SQL Commands

Types of SQL commands:
1. DDL (data definition language): defining relation schema.<br/> 
   CREATE: create table, DB, view.<br/> 
   ALTER TABLE: modification in table structure. e.g, change column datatype or add/remove columns.<br/> 
   DROP: delete table, DB, view.<br/>
2. DRL/DQL (data retrieval language / data query language): retrieve data from the tables.<br/> 
   SELECT<br/> 
3. DML (data modification language): use to perform modifications in the DB<br/> 
   INSERT: insert data into a relation<br/> 
   UPDATE: update relation data.<br/> 
   DELETE: delete row(s) from the relation.<br/> 
4. DCL (Data Control language): grant or revoke authorities from user. <br/> 
   GRANT: access privileges to the DB <br/> 
   REVOKE: revoke user access privileges.<br/> 
5. TCL (Transaction control language): to manage transactions done in the DB<br/> 
   START TRANSACTION: begin a transaction<br/> 
   COMMIT: apply all the changes and end transaction<br/> 
   ROLLBACK: discard changes and end transaction<br/> 

### CREATE COMMAND

``` sql
CREATE DATABASE ORG;
```

``` sql
SHOW DATABASES;
```

![image](https://github.com/user-attachments/assets/c89418ef-5fb9-46d4-9bc6-6402e20998da)

``` sql
USE ORG;
```

``` sql
CREATE TABLE Worker (
WORKER_ID INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
FIRST_NAME CHAR(25),
LAST_NAME CHAR(25),
SALARY INT(15),
JOINING_DATE DATETIME,
DEPARTMENT CHAR(25)
);
```
``` sql
INSERT INTO Worker
(WORKER_ID, FIRST_NAME, LAST_NAME, SALARY, JOINING_DATE, DEPARTMENT) VALUES
(001, 'Monika', 'Arora', 100000, '14-02-20 09.00.00', 'HR'),
(002, 'Niharika', 'Verma', 80000, '14-06-11 09.00.00', 'Admin'),
(003, 'Vishal', 'Singhal', 300000, '14-02-20 09.00.00', 'HR'),
(004, 'Amitabh', 'Singh', 500000, '14-02-20 09.00.00', 'Admin'),
(005, 'Vivek', 'Bhati', 500000, '14-06-11 09.00.00', 'Admin'),
(006, 'Vipul', 'Diwan', 200000, '14-06-11 09.00.00', 'Account'),
(007, 'Satish', 'Kumar', 75000, '14-01-20 09.00.00', 'Account'),
(008, 'Geetika', 'Chauhan', 90000, '14-04-11 09.00.00', 'Admin');
```
``` sql
SELECT * FROM Worker;
```
<img width="635" alt="Screenshot 2024-09-20 at 12 20 50 PM" src="https://github.com/user-attachments/assets/e15c2bd1-847c-4ae4-8fde-ff9d86a9b937">


``` sql
CREATE TABLE Bonus (
WORKER_REF_ID INT,
BONUS_AMOUNT INT(10),
BONUS_DATE DATETIME,
FOREIGN KEY (WORKER_REF_ID)
REFERENCES Worker(WORKER_ID)

ON DELETE CASCADE
);
```
``` sql
INSERT INTO Bonus (WORKER_REF_ID, BONUS_AMOUNT, BONUS_DATE) VALUES
(001, 5000, '16-02-20'),
(002, 3000, '16-06-11'),
 (003, 4000, '16-02-20'),
(001, 4500, '16-02-20'),
(002, 3500, '16-06-11');
```
### DELETE COMMAND
``` sql
DELETE FROM WORKER WHERE WORKER_ID =001;
```

![image](https://github.com/user-attachments/assets/cfbeda29-3f40-4377-a830-1f005de93fef)

``` sql
CREATE TABLE Title (
	WORKER_REF_ID INT,
	WORKER_TITLE CHAR(25),
	AFFECTED_FROM DATETIME,
	FOREIGN KEY (WORKER_REF_ID)
		REFERENCES Worker(WORKER_ID)
        ON DELETE CASCADE
);
```

``` sql
INSERT INTO Title
(WORKER_REF_ID, WORKER_TITLE, AFFECTED_FROM) VALUES
(001, 'Manager', '2016-02-20 00:00:00'),
(002, 'Executive', '2016-06-11 00:00:00'),
(008, 'Executive', '2016-06-11 00:00:00'),
(005, 'Manager', '2016-06-11 00:00:00'),
(004, 'Asst. Manager', '2016-06-11 00:00:00'),
(007, 'Executive', '2016-06-11 00:00:00'),
(006, 'Lead', '2016-06-11 00:00:00'),
(003, 'Lead', '2016-06-11 00:00:00');
```

### ALTER COMMAND 
``` sql
ALTER TABLE Worker
ADD EMAIL VARCHAR(50);
```
![image](https://github.com/user-attachments/assets/aa8b5500-219d-4c03-a255-3c41cfeb1f89)

``` sql
ALTER TABLE Worker
DROP COLUMN EMAIL;
```
![image](https://github.com/user-attachments/assets/ae695d8c-4af7-4e55-a041-c368158eedaa)

### UPDATE COMMAND
``` sql
UPDATE Worker
SET SALARY = 120000
WHERE WORKER_ID = 001;
```

![image](https://github.com/user-attachments/assets/23d48f7f-cf5d-4167-b729-ec1dfe8cc46f)


### Practice questions on SELECT query using above created tables

## Advanced SQL Queries

### Joins
* left join
``` sql
select worker.* from worker left join worker_clone using(worker_id) WHERE
worker_clone.worker_id is NULL;
```
* inner join
``` sql
select w.* from worker as w inner join title as t on w.worker_id = t.worker_ref_id where
t.worker_title = &#39;Manager&#39;;
```

``` sql
select worker.* from worker inner join worker_clone using(worker_id);
```

### Set Operation

1. UNION
* Combines two or more SELECT statements.
* SELECT * FROM table1 UNION
SELECT * FROM table2;
* Number of column, order of column must be same for table1 and table2.
2. INTERSECT
* Returns common values of the tables.
* SELECT DISTINCT column-list FROM table-1 INNER JOIN table-2 USING(join_cond);
* SELECT DISTINCT * FROM table1 INNER JOIN table2 ON USING(id);
3. MINUS
* This operator returns the distinct row from the first table that does not occur in the second table.
* SELECT column_list FROM table1 LEFT JOIN table2 ON condition WHERE table2.column_name IS NULL;
* e.g., SELECT id FROM table-1 LEFT JOIN table-2 USING(id) WHERE table-2.id IS NULL;

### SUB QUERIES and corelated queries
1. Outer query depends on inner query.
2. Alternative to joins.
3. Nested queries.
4. SELECT column_list (s) FROM table_name WHERE column_name OPERATOR
(SELECT column_list (s) FROM table_name [WHERE]);
5. e.g., SELECT * FROM table1 WHERE col1 IN (SELECT col1 FROM table1);
6. Sub queries exist mainly in 3 clauses
1. Inside a WHERE clause.
JOIN SET Operations
Combines multiple tables based on matching
condition.

Combination is resulting set from two or more
SELECT statements.
Column wise combination. Row wise combination.
Data types of two tables can be different. Datatypes of corresponding columns from each

table should be the same.
Can generate both distinct or duplicate rows. Generate distinct rows.
The number of column(s) selected may or may not
be the same from each table.

The number of column(s) selected must be the
same from each table.
Combines results horizontally. Combines results vertically.
CodeHelp

2. Inside a FROM clause.
3. Inside a SELECT clause.
7. Subquery using FROM clause
1. SELECT MAX(rating) FROM (SELECT * FROM movie WHERE country = ‘India’) as temp;
8. Subquery using SELECT
1. SELECT (SELECT column_list(s) FROM T_name WHERE condition), columnList(s) FROM T2_name WHERE
condition;
9. Derived Subquery
1. SELECT columnLists(s) FROM (SELECT columnLists(s) FROM table_name WHERE [condition]) as new_table_name;
10. Co-related sub-queries
1. With a normal nested subquery, the inner SELECT query
runs first and executes once, returning values to be used by
the main query. A correlated subquery, however, executes
once for each candidate row considered by the outer query.
In other words, the inner query is driven by the outer query.
)


## 3. Basic SQL Commands

Types of SQL commands:
1. DDL (data definition language): defining relation schema.<br/> 
   CREATE: create table, DB, view.<br/> 
   ALTER TABLE: modification in table structure. e.g, change column datatype or add/remove columns.<br/> 
   DROP: delete table, DB, view.<br/>
2. DRL/DQL (data retrieval language / data query language): retrieve data from the tables.<br/> 
   SELECT<br/> 
3. DML (data modification language): use to perform modifications in the DB<br/> 
   INSERT: insert data into a relation<br/> 
   UPDATE: update relation data.<br/> 
   DELETE: delete row(s) from the relation.<br/> 
4. DCL (Data Control language): grant or revoke authorities from user. <br/> 
   GRANT: access privileges to the DB <br/> 
   REVOKE: revoke user access privileges.<br/> 
5. TCL (Transaction control language): to manage transactions done in the DB<br/> 
   START TRANSACTION: begin a transaction<br/> 
   COMMIT: apply all the changes and end transaction<br/> 
   ROLLBACK: discard changes and end transaction<br/> 

### CREATE COMMAND

``` sql
CREATE DATABASE ORG;
```

``` sql
SHOW DATABASES;
```

![image](https://github.com/user-attachments/assets/c89418ef-5fb9-46d4-9bc6-6402e20998da)

``` sql
USE ORG;
```

``` sql
CREATE TABLE Worker (
WORKER_ID INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
FIRST_NAME CHAR(25),
LAST_NAME CHAR(25),
SALARY INT(15),
JOINING_DATE DATETIME,
DEPARTMENT CHAR(25)
);
```
``` sql
INSERT INTO Worker
(WORKER_ID, FIRST_NAME, LAST_NAME, SALARY, JOINING_DATE, DEPARTMENT) VALUES
(001, 'Monika', 'Arora', 100000, '14-02-20 09.00.00', 'HR'),
(002, 'Niharika', 'Verma', 80000, '14-06-11 09.00.00', 'Admin'),
(003, 'Vishal', 'Singhal', 300000, '14-02-20 09.00.00', 'HR'),
(004, 'Amitabh', 'Singh', 500000, '14-02-20 09.00.00', 'Admin'),
(005, 'Vivek', 'Bhati', 500000, '14-06-11 09.00.00', 'Admin'),
(006, 'Vipul', 'Diwan', 200000, '14-06-11 09.00.00', 'Account'),
(007, 'Satish', 'Kumar', 75000, '14-01-20 09.00.00', 'Account'),
(008, 'Geetika', 'Chauhan', 90000, '14-04-11 09.00.00', 'Admin');
```
``` sql
SELECT * FROM Worker;
```
<img width="635" alt="Screenshot 2024-09-20 at 12 20 50 PM" src="https://github.com/user-attachments/assets/e15c2bd1-847c-4ae4-8fde-ff9d86a9b937">


``` sql
CREATE TABLE Bonus (
WORKER_REF_ID INT,
BONUS_AMOUNT INT(10),
BONUS_DATE DATETIME,
FOREIGN KEY (WORKER_REF_ID)
REFERENCES Worker(WORKER_ID)

ON DELETE CASCADE
);
```
``` sql
INSERT INTO Bonus (WORKER_REF_ID, BONUS_AMOUNT, BONUS_DATE) VALUES
(001, 5000, '16-02-20'),
(002, 3000, '16-06-11'),
 (003, 4000, '16-02-20'),
(001, 4500, '16-02-20'),
(002, 3500, '16-06-11');
```
### DELETE COMMAND
``` sql
DELETE FROM WORKER WHERE WORKER_ID =001;
```

![image](https://github.com/user-attachments/assets/cfbeda29-3f40-4377-a830-1f005de93fef)

``` sql
CREATE TABLE Title (
	WORKER_REF_ID INT,
	WORKER_TITLE CHAR(25),
	AFFECTED_FROM DATETIME,
	FOREIGN KEY (WORKER_REF_ID)
		REFERENCES Worker(WORKER_ID)
        ON DELETE CASCADE
);
```

``` sql
INSERT INTO Title
(WORKER_REF_ID, WORKER_TITLE, AFFECTED_FROM) VALUES
(001, 'Manager', '2016-02-20 00:00:00'),
(002, 'Executive', '2016-06-11 00:00:00'),
(008, 'Executive', '2016-06-11 00:00:00'),
(005, 'Manager', '2016-06-11 00:00:00'),
(004, 'Asst. Manager', '2016-06-11 00:00:00'),
(007, 'Executive', '2016-06-11 00:00:00'),
(006, 'Lead', '2016-06-11 00:00:00'),
(003, 'Lead', '2016-06-11 00:00:00');
```

### ALTER COMMAND 
``` sql
ALTER TABLE Worker
ADD EMAIL VARCHAR(50);
```
![image](https://github.com/user-attachments/assets/aa8b5500-219d-4c03-a255-3c41cfeb1f89)

``` sql
ALTER TABLE Worker
DROP COLUMN EMAIL;
```
![image](https://github.com/user-attachments/assets/ae695d8c-4af7-4e55-a041-c368158eedaa)

### UPDATE COMMAND
``` sql
UPDATE Worker
SET SALARY = 120000
WHERE WORKER_ID = 001;
```

![image](https://github.com/user-attachments/assets/23d48f7f-cf5d-4167-b729-ec1dfe8cc46f)


### Practice questions on SELECT query using above created tables

## Advanced SQL Queries

### Joins
* left join
``` sql
select worker.* from worker left join worker_clone using(worker_id) WHERE
worker_clone.worker_id is NULL;
```
* inner join
``` sql
select w.* from worker as w inner join title as t on w.worker_id = t.worker_ref_id where
t.worker_title = &#39;Manager&#39;;
```

``` sql
select worker.* from worker inner join worker_clone using(worker_id);
```

### Set Operation

1. UNION
* Combines two or more SELECT statements.
* SELECT * FROM table1 UNION
SELECT * FROM table2;
* Number of column, order of column must be same for table1 and table2.
2. INTERSECT
* Returns common values of the tables.
* SELECT DISTINCT column-list FROM table-1 INNER JOIN table-2 USING(join_cond);
* SELECT DISTINCT * FROM table1 INNER JOIN table2 ON USING(id);
3. MINUS
* This operator returns the distinct row from the first table that does not occur in the second table.
* SELECT column_list FROM table1 LEFT JOIN table2 ON condition WHERE table2.column_name IS NULL;
* e.g., SELECT id FROM table-1 LEFT JOIN table-2 USING(id) WHERE table-2.id IS NULL;

### SUB QUERIES and corelated queries
1. Outer query depends on inner query.
2. Alternative to joins.
3. Nested queries.
4. SELECT column_list (s) FROM table_name WHERE column_name OPERATOR
(SELECT column_list (s) FROM table_name [WHERE]);
5. e.g., SELECT * FROM table1 WHERE col1 IN (SELECT col1 FROM table1);
6. Sub queries exist mainly in 3 clauses
1. Inside a WHERE clause.
JOIN SET Operations
Combines multiple tables based on matching
condition.

Combination is resulting set from two or more
SELECT statements.
Column wise combination. Row wise combination.
Data types of two tables can be different. Datatypes of corresponding columns from each

table should be the same.
Can generate both distinct or duplicate rows. Generate distinct rows.
The number of column(s) selected may or may not
be the same from each table.

The number of column(s) selected must be the
same from each table.
Combines results horizontally. Combines results vertically.
CodeHelp

2. Inside a FROM clause.
3. Inside a SELECT clause.
7. Subquery using FROM clause
1. SELECT MAX(rating) FROM (SELECT * FROM movie WHERE country = ‘India’) as temp;
8. Subquery using SELECT
1. SELECT (SELECT column_list(s) FROM T_name WHERE condition), columnList(s) FROM T2_name WHERE
condition;
9. Derived Subquery
1. SELECT columnLists(s) FROM (SELECT columnLists(s) FROM table_name WHERE [condition]) as new_table_name;
10. Co-related sub-queries
1. With a normal nested subquery, the inner SELECT query
runs first and executes once, returning values to be used by
the main query. A correlated subquery, however, executes
once for each candidate row considered by the outer query.
In other words, the inner query is driven by the outer query.
