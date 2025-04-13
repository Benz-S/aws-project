# Lab_5_RDS_Documentation

Subjects: Project Doc

# Lab 5 Documentation: Build Your DB Server and Interact With Your DB Using an App

## 📅 Lab Overview

This lab focused on using **Amazon RDS** to deploy a **MySQL** database instance and connect it to a web application hosted on an EC2 instance. The key concepts included launching an RDS instance with high availability, setting up proper networking and security configurations, and verifying database interaction via a browser-based app.

---

## 📊 Objectives Completed

- Created an **Amazon RDS DB instance**
- Created and applied a **security group** to allow connections from the web server
- Created a **DB subnet group** with subnets in two availability zones
- Interacted with the database via a **web application**

---

## 🏠 Tasks Performed

### ✅ Task 1: Create a Security Group for RDS DB Instance

- Service: **VPC**
- Created Security Group:
    - **Name**: DB Security Group
    - **Description**: Permit access from Web Security Group
    - **VPC**: Lab VPC
- Inbound Rule:
    - **Type**: MySQL/Aurora (3306)
    - **Source**: Web Security Group

### ✅ Task 2: Create a DB Subnet Group

- Service: **RDS**
- Subnet Group Created:
    - **Name**: DB-Subnet-Group
    - **Description**: DB Subnet Group
    - **VPC**: Lab VPC
    - **AZs Selected**: us-east-1a, us-east-1b
    - **Subnets Used**: 10.0.1.0/24, 10.0.3.0/24

### ✅ Task 3: Create Amazon RDS DB Instance

- DB Engine: **MySQL**
- Template: **Dev/Test**
- Multi-AZ Enabled
- Settings:
    - **Identifier**: lab-db
    - **Master Username**: main
    - **Password**: lab-password
    - **DB Class**: db.t3.micro
    - **Storage**: 20 GB (General Purpose SSD)
    - **Initial DB Name**: lab
- Networking:
    - **VPC**: Lab VPC
    - **Security Group**: DB Security Group
- Other Configurations:
    - Enhanced Monitoring: Disabled
    - Automatic Backups: Disabled
    - Encryption: Disabled
- Endpoint saved for later use

### ✅ Task 4: Interact with Your Database

- Accessed web server using provided IP
- Opened web app and navigated to **RDS** tab
- Entered DB details:
    - **Endpoint**: [copied from lab-db]
    - **DB Name**: lab
    - **Username**: main
    - **Password**: lab-password
- Successfully connected to DB
- Web app displayed address book UI
- Tested adding, editing, and deleting contacts (data persisted)

---

## ⚠️ Mistakes & Fixes

| Step | Issue | Solution |
| --- | --- | --- |
| RDS Setup | Forgot to uncheck **Enhanced Monitoring** | Received IAM error, went back and unchecked it |
| Security Group | Used 0.0.0.0/0 instead of Web Security Group | Edited rule to manually select Web Security Group |
| Web App Config | Wrong endpoint pasted | Double-checked and replaced with correct RDS value |

---