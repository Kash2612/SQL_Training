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



### Denormalization and when to use it
Denormalization is the process of adding precomputed redundant data to an otherwise normalized relational database to improve read performance. <br/>
A denormalized database should not be confused with a database that has never been normalized.
This can help us avoid costly joins in a relational database. Note that denormalization does not mean ‘reversing normalization’ or ‘not to normalize’. It is an optimization technique that is applied after normalization.


* Pros of Denormalization:

Retrieving data is faster since we do fewer joins
Queries to retrieve can be simpler(and therefore less likely to have bugs), since we need to look at fewer tables.

* Cons of Denormalization:

Updates and inserts are more expensive.
Denormalization can make update and insert code harder to write.
Data may be inconsistent.
Data redundancy necessitates more storage.


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

## 4. Advanced SQL Queries

### Joins
NOTE: <br/>
Natural Join in SQL joins two tables based on the same attribute name and datatypes. The resulting table will contain all the attributes of both tables but keep only one copy of each common column. <br/>
Inner Join joins two table on the basis of the column which is explicitly specified in the ON clause. The resulting table will contain all the attributes from both the tables including common column also. 
* inner join
``` sql
select w.* from worker as w inner join title as t on w.worker_id = t.worker_ref_id where
t.worker_title = &#39;Manager&#39;;
```

``` sql
select worker.* from worker inner join worker_clone using(worker_id);
```
- Outer Join
* left join
``` sql
select worker.* from worker left join worker_clone using(worker_id) WHERE
worker_clone.worker_id is NULL;
```
* right join
``` sql
SELECT *
FROM Worker
RIGHT JOIN Bonus ON Worker.WORKER_ID = Bonus.WORKER_REF_ID;
```
![image](https://github.com/user-attachments/assets/4cb76ac0-52da-4337-99fd-5bb476a5452c)

* full join
MySQL does not directly support FULL OUTER JOIN. However, we can achieve the same result by combining a LEFT JOIN and a RIGHT JOIN using a UNION.
``` sql
SELECT *
FROM Worker
LEFT JOIN Title ON Worker.WORKER_ID = Title.WORKER_REF_ID

UNION

SELECT *
FROM Worker
RIGHT JOIN Title ON Worker.WORKER_ID = Title.WORKER_REF_ID;
```
![image](https://github.com/user-attachments/assets/8444c7fc-cecb-4dee-bc28-d6844612d54d)

* cross join
``` sql
SELECT *
FROM Worker
CROSS JOIN Bonus;
```
![image](https://github.com/user-attachments/assets/dbb31344-4fee-41d8-958b-94a3546e69f2)

### SUB QUERIES and corelated queries
1. subqueries
* Outer query depends on inner query.
* Alternative to joins.
* Sub queries exist mainly in 3 clauses
1. Inside a WHERE clause.
``` sql
SELECT *
FROM Worker
WHERE SALARY > (SELECT AVG(SALARY) FROM Worker);
```
![image](https://github.com/user-attachments/assets/29acb6df-b50b-45dd-8f1e-f2a111bbbe63)

2. Inside a FROM clause.
``` sql
SELECT W.FIRST_NAME, W.LAST_NAME, BonusCounts.TotalBonuses
FROM Worker W
JOIN (
    SELECT WORKER_REF_ID, COUNT(*) AS TotalBonuses
    FROM Bonus
    GROUP BY WORKER_REF_ID
) AS BonusCounts ON W.WORKER_ID = BonusCounts.WORKER_REF_ID;
```
![image](https://github.com/user-attachments/assets/ced43c7b-151f-406a-8eec-fd43ec279cd5)

3. Inside a SELECT clause.
``` sql
SELECT W.FIRST_NAME, W.LAST_NAME,
       (SELECT SUM(BONUS_AMOUNT) 
        FROM Bonus 
        WHERE WORKER_REF_ID = W.WORKER_ID) AS TotalBonus
FROM Worker W;
```
![image](https://github.com/user-attachments/assets/ebf51f01-6aae-4b3e-b101-c97cf5e65e1d)




2. Co-related sub-queries
With a normal nested subquery, the inner SELECT query runs first and executes once, returning values to be used by the main query. A correlated subquery, however, executes once for each candidate row considered by the outer query. In other words, the inner query is driven by the outer query.

``` sql
SELECT FIRST_NAME, LAST_NAME
FROM Worker W1
WHERE SALARY > (
    SELECT AVG(SALARY)
    FROM Worker W2
    WHERE W1.DEPARTMENT = W2.DEPARTMENT
);
```
![image](https://github.com/user-attachments/assets/2b745f29-2e4a-47ec-97d6-7c5566c2e281)



### Set Operation

1. UNION
* Combines two or more SELECT statements.
* SELECT * FROM table1 UNION
SELECT * FROM table2;
* Number of column, order of column must be same for table1 and table2.
``` sql
SELECT FIRST_NAME FROM Worker
UNION
SELECT WORKER_TITLE FROM Title;
```
<img width="203" alt="Screenshot 2024-09-22 at 6 13 52 PM" src="https://github.com/user-attachments/assets/b76b50e5-a183-4266-8722-8ec2de14d616">

2. INTERSECT
* Returns common values of the tables.
* SELECT DISTINCT column-list FROM table-1 INNER JOIN table-2 USING(join_cond);
``` sql
SELECT W.FIRST_NAME
FROM Worker W
INNER JOIN Title T ON W.WORKER_ID = T.WORKER_REF_ID;
```
![image](https://github.com/user-attachments/assets/90bfea92-b4a9-45d6-8905-5fba24b1d8c8)


3. MINUS
* This operator returns the distinct row from the first table that does not occur in the second table.
* SELECT column_list FROM table1 LEFT JOIN table2 ON condition WHERE table2.column_name IS NULL;

``` sql
INSERT INTO Worker (FIRST_NAME, LAST_NAME, SALARY, JOINING_DATE, DEPARTMENT) VALUES
('John', 'Doe', 60000, '2023-01-01 09:00:00', 'HR');
```

``` sql
SELECT W.FIRST_NAME
FROM Worker W
LEFT JOIN Title T ON W.WORKER_ID = T.WORKER_REF_ID
WHERE T.WORKER_REF_ID IS NULL;
```
![image](https://github.com/user-attachments/assets/ca03f7a0-4d94-4147-8fe9-99dfa7d6567f)


### Window functions
1. ROW_NUMBER(): Unique sequential number for rows.
``` sql
SELECT FIRST_NAME, SALARY,
       ROW_NUMBER() OVER (ORDER BY SALARY DESC) AS RowNum
FROM Worker;
```
![image](https://github.com/user-attachments/assets/31d81a9e-83ba-4f91-87e5-e0f54e8621ac)

2. RANK(): Rank with gaps for ties.
``` sql
SELECT FIRST_NAME, SALARY,
       RANK() OVER (ORDER BY SALARY DESC) AS Rank_Number
FROM Worker;
```
![image](https://github.com/user-attachments/assets/9b610d0f-ab57-4153-9506-a09323714d6d)

3. DENSE_RANK(): Rank without gaps for ties.
``` sql
SELECT FIRST_NAME, SALARY,
       DENSE_RANK() OVER (ORDER BY SALARY DESC) AS DenseRank
FROM Worker;
```
![image](https://github.com/user-attachments/assets/f0180ae7-643f-48ef-aff3-eb7ef9245028)

4. NTILE(): Divide the result set into specified groups.
``` sql
SELECT FIRST_NAME, SALARY,
       NTILE(4) OVER (ORDER BY SALARY DESC) AS SalaryQuartile
FROM Worker;
```
![image](https://github.com/user-attachments/assets/8098009a-7a20-4d55-ae7a-61ebe11e2b7b)

5. LAG(): Access data from the previous row.
``` sql
SELECT FIRST_NAME, SALARY,
       LAG(SALARY, 1) OVER (ORDER BY SALARY DESC) AS PreviousSalary
FROM Worker;
```
![image](https://github.com/user-attachments/assets/9bb6151c-b0d7-467c-a17a-b88607a29787)

6. LEAD(): Access data from the next row.
``` sql
SELECT FIRST_NAME, SALARY,
       LEAD(SALARY, 1) OVER (ORDER BY SALARY DESC) AS NextSalary
FROM Worker;
```
![image](https://github.com/user-attachments/assets/0221242b-6744-466f-95f4-ab12460fc485)

### Pivoting and unpivoting data
``` sql
SELECT 
    W.FIRST_NAME,
    SUM(CASE WHEN B.BONUS_DATE = '2016-02-20' THEN B.BONUS_AMOUNT ELSE 0 END) AS Bonus_Feb_2016,
    SUM(CASE WHEN B.BONUS_DATE = '2016-06-11' THEN B.BONUS_AMOUNT ELSE 0 END) AS Bonus_Jun_2016
FROM 
    Worker W
LEFT JOIN 
    Bonus B ON W.WORKER_ID = B.WORKER_REF_ID
GROUP BY 
    W.FIRST_NAME;
```
![image](https://github.com/user-attachments/assets/90736d7d-729d-44c3-8760-8de87b25d842)



## 6. Stored Procedures and Functions

### Creating and managing stored procedures
``` sql
DELIMITER //

CREATE PROCEDURE SelectAllWorkers()
BEGIN
    SELECT * FROM Worker;
END //

DELIMITER ;
```
For Execution
``` sql
CALL SelectAllWorkers();
```

* DELIMITER: Changes the statement delimiter to allow for the creation of the procedure.
* CREATE PROCEDURE SelectAllWorkers(): Defines the stored procedure with no parameters.
* BEGIN...END: Encloses the SQL statements that belong to the procedure.
* SELECT * FROM Worker;: The SQL statement to retrieve all records from the Worker table.

![image](https://github.com/user-attachments/assets/eba7d796-26a1-446a-8fbb-9868e5257973)

* creating procedures with multiple parameters
``` sql
DELIMITER //

CREATE PROCEDURE SelectWorkersByDepartmentAndDate(
    IN departmentName VARCHAR(25), 
    IN joiningDate DATETIME
)
BEGIN
    SELECT * 
    FROM Worker 
    WHERE DEPARTMENT = departmentName AND JOINING_DATE >= joiningDate;
END //

DELIMITER ;
```
for execution
``` sql
CALL SelectWorkersByDepartmentAndDate('HR', '2020-01-01');
```
![image](https://github.com/user-attachments/assets/2da232c2-0324-478b-9f0b-984ec7111c1f)

### User-defined functions
User-defined functions in SQL Server are custom functions created by users to perform specific tasks. They can accept parameters, execute a block of SQL code, and return a single value or a table result set. SQL Server supports three types of user-defined functions:

1. Scalar Functions: These functions return a single value based on the input parameters.
2. Inline Table-Valued Functions (iTVFs): These functions return a table variable result set.
3. Multi-Statement Table-Valued Functions (mTVFs): These functions return a table result set after executing a block of SQL code.
This function calculates the annual salary of a worker based on their monthly salary.
``` sql
DELIMITER //

CREATE FUNCTION CalculateAnnualSalary(monthlySalary INT)
RETURNS INT
DETERMINISTIC
BEGIN
    RETURN monthlySalary * 12;
END //

DELIMITER ;
```
``` sql
SELECT FIRST_NAME, LAST_NAME, CalculateAnnualSalary(SALARY) AS AnnualSalary
FROM Worker;
```
![image](https://github.com/user-attachments/assets/e427c39a-2f39-4538-a86c-09dc16182307)


### Triggers and its uses

A trigger is a stored procedure in a database that automatically invokes whenever a special event in the database occurs. For example, a trigger can be invoked when a row is inserted into a specified table.<br/>
It belongs to a specific class of stored procedures that are automatically invoked in response to database server events. Every trigger has a table attached to it.<br/>
Because a trigger cannot be called directly, unlike a stored procedure, it is referred to as a special procedure. A trigger is automatically called whenever a data modification event against a table takes place, which is the main distinction between a trigger and a procedure. On the other hand, a stored procedure must be called directly.

* The following are the key differences between triggers and stored procedures:

1. Triggers cannot be manually invoked or executed.
2. There is no chance that triggers will receive parameters.
3. A transaction cannot be committed or rolled back inside a trigger.

* Step 1: Modify the Worker Table
``` sql
ALTER TABLE Worker ADD COLUMN LAST_UPDATED DATETIME;
```
* Step 2: Create the Trigger
``` sql
DELIMITER //

CREATE TRIGGER UpdateLastUpdated
BEFORE UPDATE ON Worker
FOR EACH ROW
BEGIN
    SET NEW.LAST_UPDATED = NOW();
END //

DELIMITER ;
```

Running the trigger
``` sql
UPDATE Worker SET SALARY = 120000 WHERE WORKER_ID = 1;
```

Output
``` sql
SELECT * FROM Worker WHERE WORKER_ID = 1;
```
![image](https://github.com/user-attachments/assets/b0eec839-85dc-4231-be4b-ffc55bc47bcb)

###  Error handling and transaction management within stored procedures
* Transaction Management
Transactions allow you to execute a sequence of operations as a single unit. If any operation fails, you can roll back all changes to maintain data integrity.

* Error Handling
In MySQL, you can use the DECLARE statement to define a handler for exceptions. This allows you to manage errors gracefully.

* Let's create a stored procedure
``` sql
DELIMITER //

CREATE PROCEDURE AddBonusAndUpdateTitle(
    IN workerID INT,
    IN bonusAmount INT,
    IN bonusDate DATETIME
)
BEGIN
    DECLARE totalBonus INT DEFAULT 0;

    -- Error handling for SQL exceptions
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        -- Rollback the transaction if there's an error
        ROLLBACK;
        SELECT 'Error occurred while processing the transaction.' AS Message;
    END;

    -- Start the transaction
    START TRANSACTION;

    -- Insert the bonus into the Bonus table
    INSERT INTO Bonus (WORKER_REF_ID, BONUS_AMOUNT, BONUS_DATE)
    VALUES (workerID, bonusAmount, bonusDate);

    -- Calculate total bonuses for the worker
    SELECT SUM(BONUS_AMOUNT) INTO totalBonus
    FROM Bonus
    WHERE WORKER_REF_ID = workerID;

    -- Update the title if total bonuses exceed 10,000
    IF totalBonus > 10000 THEN
        UPDATE Title
        SET WORKER_TITLE = 'Senior'
        WHERE WORKER_REF_ID = workerID;
    END IF;

    -- Commit the transaction
    COMMIT;
    SELECT 'Transaction completed successfully.' AS Message;
END //

DELIMITER ;
```
``` sql
CALL AddBonusAndUpdateTitle(2, 8000, '2024-09-22');
```
![image](https://github.com/user-attachments/assets/330c5586-553f-4099-b229-8cc0f4a70c4a)

``` sql
SELECT * FROM Bonus WHERE WORKER_REF_ID = 2;
SELECT * FROM Title WHERE WORKER_REF_ID = 2;
```

![image](https://github.com/user-attachments/assets/124952dd-bf7c-47b6-8148-4b20c7978209)

