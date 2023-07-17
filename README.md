# Cloud Project

- [Project Description](#project-description)
- [Architecture Diagram](#architecture-diagram)
- [Step by Step Guide](#step-by-step-guide)
  - [Step 1: Create a VPC](#step-1-create-a-vpc)
  - [Step 2: Create EC2 Instance](#step-2-create-ec2-instance)
  - [Step 3: Create RDS Instance](#step-3-create-rds-instance)
  - [Step 4: Setup Connection between EC2 and RDS](#step-4-setup-connection-between-ec2-and-rds)

## Project Description

This project is a simple PHP application that communicates with a MySQL database both hosted on AWS inside a VPC.

The team is composed of:

- Antoine LACHAUD
- Romain MARQUES
- Jathoosh THAVARASA

## Architecture Diagram

![Architecture Diagram](Task%201%20-%20PHP%20Site%20with%20DB/P0%20-%20Schema.jpg)

## Step by Step Guide

### Step 1: Create a VPC

We create a VPC with a CIDR block of `10.0.0.0/16`

![VPC Creation](Task%201%20-%20PHP%20Site%20with%20DB/P1-1%20-%20VPC%20Creation.png)

We also create a public subnet with a CIDR block of `10.0.0.0/24` and an Internet Gateway

![Public Subnet Creation](Task%201%20-%20PHP%20Site%20with%20DB/P1-2%20-%20Public%20Subnet%20Creation.png)
![Internet Gateway Creation](Task%201%20-%20PHP%20Site%20with%20DB/P1-3%20-%20Internet%20Gateway%20creation.png)

Finally, we allow internet traffic on the public subnet

![Allow Internet Traffic on the Public Subnet](Task%201%20-%20PHP%20Site%20with%20DB/P1-4%20-%20Allow%20internet%20traffic%20on%20the%20public%20subnet.png)

### Step 2: Create EC2 Instance

We create an EC2 instance with the following parameters:

- AMI: `Cloud9AmazonLinux2-2023-06-22T17-21`
- Instance Type: `t2.micro`
- Network: `VPC` and `Public Subnet`
- Security Group: `Allow HTTP and SSH`

![EC2 Instance Creation](Task%201%20-%20PHP%20Site%20with%20DB/P2-1%20-%20EC2%20Instance%20Creation.png)
![EC2 Instance AMI Details](Task%201%20-%20PHP%20Site%20with%20DB/P2-2%20-%20EC2%20Instance%20AMI%20details.png)

Since we was not able to directly create and connect to the EC2 instance through Cloud9, we had to create the EC2 instance. But, to access it, we had to manually create a key pair and add it to the EC2 instance authorized keys.

PS: From this step, we had the possibility to link the EC2 instance to the Cloud9 IDE, by passing Cloud9's public key to the EC2 instance through AWS CLI. However, even if the key was correctly added to the EC2 instance, we were not able to use Cloud9 since it was installing dependencies indefinitely.

![Create RSA Keys](Task%201%20-%20PHP%20Site%20with%20DB/P2-3%20-%20Create%20RSA%20Keys.png)
![Access to the EC2 Instance without Key Pairs](Task%201%20-%20PHP%20Site%20with%20DB/P2-4%20-%20Access%20to%20the%20EC2%20instance%20without%20Key%20Pairs.png)

We then update the EC2 instance inbound rules to allow HTTP traffic from anywhere through port 80.

![Update EC2 Inbound Rules for HTTP](Task%201%20-%20PHP%20Site%20with%20DB/P2-5%20-%20Update%20EC2%20Inbound%20Rules%20for%20HTTP.png)

Finally, thanks to the Internet Gateway, we are able to download the PHP website with a `wget` commandn, unzip it and let apache (httpd) serve it.

![Setup the PHP Website](Task%201%20-%20PHP%20Site%20with%20DB/P2-6%20-%20Setup%20the%20PHP%20Website.png)
![Check if the website is functional (not the query part)](Task%201%20-%20PHP%20Site%20with%20DB/P2-7%20-%20Check%20if%20the%20website%20is%20functional%20(not%20the%20query%20part).png)

### Step 3: Create RDS Instance

To create an RDS instance in a VPC, we need to create two private subnets, each one in a different availability zone.

![Create Two Private Subnets](Task%201%20-%20PHP%20Site%20with%20DB/P3-1%20-%20Create%20two%20private%20subnets.png)

Then, we create a RDS instance with the following parameters and link it to the VPC and the private subnets:

- Engine: `MariaDB`
- Version: `Latest`
- DB Instance Class: `db.t2.micro` (Free Tier)
- Storage: `20 GB`
- Network: `VPC` and `Private Subnet`
- Security Group: `Allow MySQL/Aurora (3306) to the EC2 Instance Security Group`

![Create RDS Database](Task%201%20-%20PHP%20Site%20with%20DB/P3-2%20-%20Create%20RDS%20Database.png)
![RDS linked to VPC and Privates Subnets](Task%201%20-%20PHP%20Site%20with%20DB/P3-3%20-%20RDS%20linked%20to%20VPC%20and%20Privates%20Subnets.png)

Finally, through the EC2 instance, we connect to the RDS instance by its endpoint and create the database and the table via the SQL dump file.

![Database is implemented and filled](Task%201%20-%20PHP%20Site%20with%20DB/P3-4%20-%20Database%20is%20implemented%20and%20filled.png)

### Step 4: Setup Connection between EC2 and RDS

Now, to allow the PHP website to connect to the RDS instance, we need to define required parameters in the AWS Parameter Store.

![Allow connection to the database by our EC2 Instance](Task%201%20-%20PHP%20Site%20with%20DB/P4-1%20-%20Allow%20Connection%20to%20the%20database%20by%20our%20EC2%20Instance.png)
![Create Parameters for the link between the website and database](Task%201%20-%20PHP%20Site%20with%20DB/P4-2%20-%20Create%20Parameters%20for%20the%20link%20between%20the%20website%20and%20database.png)

Finally, we need to create and add the correct IAM role for EC2 to allow it to access the AWS Parameter Store.

![Create and Add the correct IAM role for EC2](Task%201%20-%20PHP%20Site%20with%20DB/P4-3%20-%20Create%20and%20Add%20the%20correct%20IAM%20role%20for%20EC2.png)

Now, the website is fully functional and can be used.

![Everything works](Task%201%20-%20PHP%20Site%20with%20DB/P4-4%20-%20Everything%20works.png)
