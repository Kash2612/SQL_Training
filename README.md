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
