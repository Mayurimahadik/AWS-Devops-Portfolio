# AWS RDS MySQL Project

## Project Overview

This project demonstrates the creation of an Amazon RDS MySQL database and secure connectivity from an EC2 instance.

## Services Used

* Amazon EC2
* Amazon RDS (MySQL)
* VPC
* Security Groups
* Ubuntu Linux
* MySQL Client

## Architecture

EC2 Instance → Amazon RDS MySQL

## Implementation Steps

### Step 1: Launch EC2 Instance

* Launched Ubuntu EC2 instance.
* Configured Security Group for SSH access.

### Step 2: Create Amazon RDS MySQL Database

* Engine: MySQL
* Instance Type: db.t3.micro
* Created database user and password.

### Step 3: Configure Security Groups

* Allowed MySQL port 3306.
* Enabled communication between EC2 and RDS.

### Step 4: Connect EC2 to RDS

* Installed MySQL client on EC2.
* Connected to RDS using the endpoint.

### Step 5: Create Database and Table

```sql
CREATE DATABASE company;

USE company;

CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);
```

### Step 6: Insert Data

```sql
INSERT INTO employees VALUES (1,'Mayuri');
INSERT INTO employees VALUES (2,'Rahul');
INSERT INTO employees VALUES (3,'Priya');
```

### Step 7: Verify Data

```sql
SELECT * FROM employees;
```

## Skills Demonstrated

* AWS EC2
* AWS RDS
* VPC Networking
* Security Groups
* Linux Administration
* MySQL Database Management

## Outcome

Successfully deployed an Amazon RDS MySQL database, connected it with an EC2 instance, and performed database operations using SQL queries.
