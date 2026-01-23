# Infrastructure Setup: PhotoSharing App VPC
This guide outlines the steps to build a secure, high-availability network environment for the PhotoSharing application using Amazon VPC. We utilize a "Front Door" (Public) and "Vault" (Private) architecture to isolate sensitive data from the public internet.

## Architecture Overview

- Public Subnets: House the Application Load Balancer (ALB) and Web Servers.
- Private Subnets: House the Database to ensure data isolation.
- Multi-AZ Deployment: Subnets are spread across us-east-1a and us-east-1b for high availability.

## Deployment Steps

### Step 1: Build the VPC
- Navigate to the VPC Dashboard.
- Click Create VPC.
- Name tag: photoshare-vpc
- IPv4 CIDR block: 10.0.0.0/16
- Click Create.

### Step 2: Create the Subnets
Create the following four subnets within photoshare-vpc:
- [Infrastructure Setup: PhotoSharing App VPC](#infrastructure-setup-photosharing-app-vpc)
  - [Architecture Overview](#architecture-overview)
  - [Deployment Steps](#deployment-steps)
    - [Step 1: Build the VPC](#step-1-build-the-vpc)
    - [Step 2: Create the Subnets](#step-2-create-the-subnets)
    - [Step 3: Add the Internet Gateway](#step-3-add-the-internet-gateway)
    - [Step 4: Configure Routing](#step-4-configure-routing)
      - [1. Create Route Table:](#1-create-route-table)
      - [2. Add Internet Access:](#2-add-internet-access)
      - [3. Associate Public Subnets:](#3-associate-public-subnets)
  - [Post-Setup Checklist](#post-setup-checklist)


### Step 3: Add the Internet Gateway
- Go to Internet Gateways > Create internet gateway.
- Name tag: photoshare-igw
- Select the new gateway, click Actions, and choose Attach to VPC.
- Select photoshare-vpc and click Attach.

### Step 4: Configure Routing
Create a Public Route Table to enable internet access for the web tier while keeping the database tier isolated.

#### 1. Create Route Table:
- Go to Route Tables > Create route table.
- Name: public-rt
- VPC: photoshare-vpc

#### 2. Add Internet Access:
- Select public-rt > Routes tab > Edit routes.
- Destination: 0.0.0.0/0
- Target: Internet Gateway (Select photoshare-igw).
- Click Save changes.

#### 3. Associate Public Subnets:
- Click Subnet associations tab > Edit subnet associations.
- Select Public Subnet 1 and Public Subnet 2.
- Ensure Private Subnets remain unchecked.
- Click Save associations.

## Post-Setup Checklist

- VPC Created: Is photoshare-vpc active?
- Subnet Layout: Are all 4 subnets created correctly across AZs 1a and 1b?
- Connectivity: Do both Public Subnets have a route to 0.0.0.0/0 via the IGW?
- Isolation: Are the Private Subnets excluded from the Public Route Table?
