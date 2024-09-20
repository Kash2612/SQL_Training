# SQL_Training

## 1. Introduction to Databases
* Types of databases: Relational vs. Non-relational
Relational databases store data in rows and columns like a spreadsheet while non-relational databases don't, using a storage model that is best suited for the type of data it’s storing.

## 3. Basic SQL Commands

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

``` sql
DELETE FROM WORKER WHERE WORKER_ID =001;
```

![image](https://github.com/user-attachments/assets/cfbeda29-3f40-4377-a830-1f005de93fef)
