
#  EC2 To AWS RDS Data Migration

## Description

This project demonstrates a traditional database migration from a self-managed MariaDB instance running on Amazon EC2 to a fully managed Amazon RDS MariaDB instance. It includes EC2 database setup, backup creation using mysqldump, and seamless restoration to RDS. This approach is well suited for lift-and-shift migrations, helping organizations improve scalability, reliability, and operational efficiency by moving from self-managed infrastructure to managed AWS database services.

---

## Architecture Overview

![alt text](./img/architecture-diagram.png)

This workflow outlines the end-to-end process of migrating a MariaDB database from EC2 to Amazon RDS. It includes installing and configuring MariaDB on the EC2 instance, creating and populating the source database, generating a consistent backup using mysqldump, and restoring the data into the RDS MariaDB instance. Secure connectivity between EC2 and RDS is ensured through correctly configured security group rules and MariaDB client access, enabling a smooth and reliable migration.

Key Workflow:

- Configure MariaDB on EC2

- Create and populate the source database

- Export the database using mysqldump

- Import and validate data in Amazon RDS

---


## Prerequisites

- AWS EC2 instance with SSH access
- AWS RDS MariaDB instance 
- Security Group allowing:
  - Port 22 (SSH)
  - Port 3306 (MariaDB)
- RDS endpoint, username, and password

---

## Step 1: Install MariaDB on EC2

```bash
#!/bin/bash
sudo yum update -y
sudo yum install mariadb105-server -y
sudo systemctl start mariadb
sudo systemctl enable mariadb
````
![alt text](./img/db-server.png)

![alt text](./img/mariad-db-running.png)
---

## Step 2: Configure Traditional MariaDB Ec2 and Create Database

```bash
sudo mysql -u root
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY <password>;
SHOW DATABASES;

CREATE DATABASE student_db;
SHOW DATABASES;

USE student_db;
CREATE TABLE user (
  id INT(10),
  name VARCHAR(20),
  city VARCHAR(20)
);

INSERT INTO user VALUES
(1, 'Uttam', 'Sangli'),
(2, 'Rohan', 'Kolhapur'),
(3, 'Aniket', 'Satara');

SELECT * FROM user;
```
![alt text](./img/data-in-mariadb.png)
---

## Step 3: Backup Traditional Database

```bash
sudo mysqldump -u root -p student_db > studentbackupfile.sql
```
![alt text](./img/dump-command.png)
---

## Step 4.1: Create RDS instance

Create RDS using following configurations:

![alt text](./img/mariadb-selection.png)

![alt text](./img/rds.png)



## Step 4.2: Connect to AWS RDS to EC2

```bash
sudo mysql -h <rds-endpoint> -u <rds-username> -p
```

Create a new database in RDS:

```sql
CREATE DATABASE student_db;
```
---

## Step 5: Restore Backup to RDS

```bash
sudo mysql -h <rds-endpoint> -u <rds-username> -p student_db < studentbackupfile.sql
```
![alt text](./img/rds-access.png)

---

## Step 6: Verify Migration on RDS

```sql
SHOW DATABASES;
USE student_db;
SHOW TABLES;
SELECT * FROM user;
```
![alt text](./img/data-migrate-in%20rds.png)
---

## Summary

| Step             | Purpose                                                                 |
|------------------|-------------------------------------------------------------------------|
| Local DB Setup   | Install, configure, and populate the MariaDB database on the EC2 host |
| mysqldump Export | Generate a consistent logical backup of the source database            |
| RDS Import       | Restore the backup into the Amazon RDS MariaDB instance                |
| Validation       | Verify schema, tables, and data integrity after migration              |

---
## ⭐ Conclusion

This project successfully demonstrates migration of a local MariaDB database from EC2 to Amazon RDS, ensuring a smooth transition from self-managed databases to managed AWS database services.

## Contact

Author: Aniket Yewale

Email: aniketyewale626@gmail.com

LinkedIn: [www.linkedin.com/in/aniket-yewale-0317a33a6](www.linkedin.com/in/aniket-yewale-0317a33a6)