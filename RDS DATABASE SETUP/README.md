# RDS Database Setup
Every photo needs data attached to it, such as who uploaded it, when, and what the title is. We need a reliable database to store this information. We use Amazon RDS to run a managed MySQL database. Crucially, we place this database in the Private Subnet. This means it is completely invisible to the public internet. Only our Web Server (EC2) can talk to it, making it extremely difficult for attackers to try and guess passwords or steal data.

## Tasks:

### Step 1: Create DB Subnet Group

Go to the RDS Dashboard and create a new DB Subnet Group.

- Name: photoshare-db-group
- Description: DB Subnet Group for PhotoShare
- VPC: Select photoshare-vpc
- Availability Zones: us-east-1a, us-east-1b
- Subnets: Select the Private Subnets (10.0.2.0/24 and 10.0.4.0/24)

### Step 2: Create Security Group for Database

Create a new Security Group for the database.

- Name: db-sg
- Description: Security group for PhotoShare RDS database
- VPC: photoshare-vpc
- Configure Inbound Rules:

- Type: MySQL/Aurora
- Protocol: TCP
- Port: 3306
- Source: 10.0.0.0/16 (Entire VPC)
  
### Step 3: Create RDS MySQL Instance

Go to RDS Instances and create a new MySQL database instance.

- Engine: MySQL
- Engine Version: 8.4 (or latest 8.4.x)
- Templates: Free Tier
- DB Instance Identifier: photoshare-db
- Master Username: admin
- Master Password: Create a strong password and save it
- DB Instance Class: db.t3.micro (Free Tier eligible)
- Storage: 20 GB (Free Tier)
- Storage Type: gp3
- DB Subnet Group: photoshare-db-group
- Public Access: No
- VPC Security Groups: db-sg
- Disable Automated Backup
- Multi-AZ: No (for Free Tier)
- Advance Configuration: Initial Database Name photoshare

## Note:
- Is the RDS instance photoshare-db status Available?
- Is the database Public Access set to No?