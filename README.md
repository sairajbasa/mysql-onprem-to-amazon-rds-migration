## 🚀 MySQL On-Prem (EC2) to Amazon RDS Migration Lab

### 📌 Project Overview

This project demonstrates a hands-on database migration from a simulated on-prem MySQL server (running on EC2 Linux) to Amazon RDS using logical backup and restore.

The lab includes:
- Installing and securing MySQL
- Creating database and tables
- Performing logical backup using mysqldump
- Restoring into Amazon RDS
- Verifying migration success using MySQL Workbench
- Troubleshooting real-world migration issues


### 🏗 Architecture

```text
+-----------------------------+
|  On-Prem MySQL (EC2 Linux)  |
+-------------+---------------+
              |
              v
+-----------------------------+
|  Logical Backup (mysqldump) |
+-------------+---------------+
              |
              v
+-----------------------------+
|  Amazon RDS (MySQL Engine)  |
|         Restoring           |
+-------------+---------------+
              |
              v
+-----------------------------+
|  Validation (MySQL Workbench) |
+-----------------------------+
```

### 🛠 Technologies Used

- Linux (RHEL-based)
- MySQL 8.4
- Amazon RDS
- Amazon EC2
- MySQL Workbench

---

🔧 Step 1: Install MySQL on EC2

```bash
dnf module install mysql:8.4/server
systemctl enable --now mysqld
mysql --version
```
Secure installation:
```mysql
mysql_secure_installation
```

During the secure setup, the following actions were performed:
- Set a strong root password
- Removed anonymous users
- Removed test database
- Reloaded privilege tables
- Disabled remote root login for security

Why Disable Remote Root Login?
Disabling remote root login ensures that the MySQL root user can only connect locally from the server itself.

This follows security best practices by:

- Preventing remote administrative access
- Reducing attack surface
- Enforcing least privilege principle


🗄 Step 2: Create Database, User & Tables

Login:
```bash
mysql -u root -p
```
After Login into DBServer:
```mysql
Create database and user;

create database testdb;

create user 'labuser'@'%' identified by 'StrongPassword';

GRANT ALL PRIVILEGES ON testdb.* TO 'labuser'@'%';

FLUSH PRIVILEGES;
```


Create sample tables:
```mysql
show databases;

use testdb;

create table employees (
    id int primary key auto_increment,
    name varchar(100),
    department varchar(100),
    salary decimal(10,2)
);

create table departments (
    dept_id int primary key auto_increment,
    dept_name varchar(100)
);


Insert sample data:

insert into employees (name, department, salary)
values ('John Doe', 'IT', 60000),
       ('Jane Smith', 'HR', 50000);

insert into departments (dept_name)
values ('IT'), ('HR');
```

💾 Step 3: Take Logical Backup
```bash
mysqldump -u root -p --single-transaction testdb > testdb_backup.sql
```

☁ Step 4: Restore to Amazon RDS
```bash
mysql -h <rds-endpoint> -u admin -p testdb < testdb_backup.sql
```

🔍 Step 5: Migration Verification
```text
Connected to RDS endpoint using MySQL Workbench
Verified testdb schema exists
Confirmed all tables were successfully migrated
Run SELECT queries to validate data integrity
```

⚠ Challenges Faced

- Access denied due to host-based authentication mismatch
- PROCESS privilege error during mysqldump
- Attempted to run mysqldump inside MySQL client
- Minor syntax issue during restore command

All issues were resolved and migration completed successfully.

🧠 Key Learnings
- MySQL privilege management
- Host-based authentication ('user'@'%')
- Logical backup consistency using --single-transaction
- Difference between MySQL CLI and Linux shell
- Amazon RDS connectivity
- Migration validation best practices

📸 Screenshots
Available in the /screenshots folder.

👨‍💻 Author
-- Sairaj
Cloud & DevOps Enthusiast 🚀
