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
(001, &#39;Monika&#39;, &#39;Arora&#39;, 100000, &#39;14-02-20 09.00.00&#39;, &#39;HR&#39;),
(002, &#39;Niharika&#39;, &#39;Verma&#39;, 80000, &#39;14-06-11 09.00.00&#39;, &#39;Admin&#39;),
(003, &#39;Vishal&#39;, &#39;Singhal&#39;, 300000, &#39;14-02-20 09.00.00&#39;, &#39;HR&#39;),
(004, &#39;Amitabh&#39;, &#39;Singh&#39;, 500000, &#39;14-02-20 09.00.00&#39;, &#39;Admin&#39;),
(005, &#39;Vivek&#39;, &#39;Bhati&#39;, 500000, &#39;14-06-11 09.00.00&#39;, &#39;Admin&#39;),
(006, &#39;Vipul&#39;, &#39;Diwan&#39;, 200000, &#39;14-06-11 09.00.00&#39;, &#39;Account&#39;),
(007, &#39;Satish&#39;, &#39;Kumar&#39;, 75000, &#39;14-01-20 09.00.00&#39;, &#39;Account&#39;),
(008, &#39;Geetika&#39;, &#39;Chauhan&#39;, 90000, &#39;14-04-11 09.00.00&#39;, &#39;Admin&#39;);
```
``` sql
SELECT * FROM Worker;
```
<img width="635" alt="Screenshot 2024-09-20 at 12 20 50 PM" src="https://github.com/user-attachments/assets/e15c2bd1-847c-4ae4-8fde-ff9d86a9b937">
