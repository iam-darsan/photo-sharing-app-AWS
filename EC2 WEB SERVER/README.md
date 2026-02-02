# EC2 WEB SERVER

This is the heart of the PhotoSharing app. We will launch this instance in the Public Subnet to allow for configuration access, but we will secure it using strict Security Groups and IAM roles.

## Tasks:

### Step 1: Create Web Server Security Group

- Security group name: photoshare-web-sg
- Description: Security group for Web Server
- VPC: photoshare-vpc
- Inbound Rules:
- Type: HTTP | Port: 80 | Source: Custom -> Select the ALB Security Group.
- Type: SSH | Port: 22 | Source: 0.0.0.0/0 (Allows SSH access for configuration).

### Step 2: Update RDS Security Group
Lock down the Database so only this specific EC2 instance can talk to it.

- Go to Security Groups -> Select db-sg.
- Edit inbound rules -> Delete existing rules.
- Add Rule:
- Type: MySQL/Aurora | Port: 3306 | Source: Custom -> Select photoshare-web-sg.
- Click Save rules.
- 
### Step 3: Launch the Instance
Go to the EC2 Dashboard and click Launch Instances.

- Name: photoshare-web
- AMI: Amazon Linux 2023
- Instance Type: t3.micro
- Key Pair: Select an existing key pair or create a new one (e.g., photoshare-key).
- Note: You need this if you want to use a standard terminal. If you plan to only use EC2 Instance Connect, you can proceed without one, but having one is recommended.
- Network Settings:
- VPC: photoshare-vpc
- Subnet: Select the Public Subnet
- Auto-assign Public IP: Enable (Required for internet access and SSH).
- Security Group: Select photoshare-web-sg.
- Advanced Details:
- IAM instance profile: Select iam_role_ec2.
  
### Step 4: Configure the Server

Prepare File Content: Run this in the terminal and copy the output to your clipboard:
cat docker-compose.yml

Connect to EC2

- Option A (Browser): Select instance -> Connect -> EC2 Instance Connect.
- Option B (SSH): Run ssh -i key.pem ec2-user@<Public-IP>
- Install, Create Files & Run (Inside EC2): Run these commands in order:

#### 1. Install Docker & Git
- sudo dnf install -y docker git
- sudo systemctl enable --now docker
- sudo usermod -aG docker ec2-user

#### 2. Install Docker Compose
- sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
- sudo chmod +x /usr/local/bin/docker-compose

#### 3. Create the docker-compose.yml file
#### Run this, PASTE your clipboard content, then press Ctrl+D.
cat > docker-compose.yml

#### 4. Create the .env file
#### You MUST replace 'photoshare-assets-xxxx' with your actual bucket name.
- cat <<EOF > .env
- S3_BUCKET=photoshare-assets-xxxx
- AWS_SECRET_NAME=photoshare/db/credentials
- EOF

#### 5. Run the application
sudo docker-compose up -d

### Step 5: Register to Target Group

- Go to Target Groups -> Select photoshare-tg.
- Click Register targets.
- Select the photoshare-web instance.
- Click Include as pending below -> Register pending targets.

## Verification:
- Is the instance running in the Public Subnet using iam_role_ec2?
- Does the RDS Security Group (db-sg) allow traffic ONLY from photoshare-web-sg?
- Action Required: Test the website via the Load Balancer DNS (e.g., http://photoshare-alb-123...)