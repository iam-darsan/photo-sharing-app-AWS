# Application Load Balancer

We don’t want to expose our Web Server directly to the internet. If it crashes or gets attacked, the whole application could go down. The Application Load Balancer (ALB) acts as our Receptionist.

It lives in the Public Subnet, accepts incoming user traffic, and safely forwards requests to the Web Server running in the backend. This setup provides a secure entry point and protects our infrastructure from direct internet exposure.

## Tasks:

### Step 1: Create the Application Load Balancer

Go to the EC2 Dashboard, select Load Balancers, and click Create Load Balancer.

- Load balancer type: Application Load Balancer
- Name: photoshare-alb
- Scheme: Internet-facing
- IP address type: IPv4
- Network mapping:
- VPC: photoshare-vpc
- Mappings: Select the Public Subnets in both zones:
- Check us-east-1a -> Select Public Subnet 1
- Check us-east-1b -> Select Public Subnet 2

## Step 2: Create the Security Group

Create a new Security Group for the ALB. photoshare-sg

Inbound rules:
- HTTP — Port 80 — Source: 0.0.0.0/0
- SSH — Port 22 — Source: 0.0.0.0/0
- Outbound rules: Allow all traffic
- Attach this Security Group to the ALB.

## Step 3: Configure the Target Group

Create a Target Group that the ALB will forward traffic to.

- Target group name: photoshare-tg
- Target type: Instance
- Protocol: HTTP
- Port: 80
- We will register the Web Server EC2 instance to the target group later.

## Verification:
- Is the ALB scheme set to Internet-facing?
- Does the Security Group allow inbound traffic from 0.0.0.0/0?
- Action Required: Copy the DNS Name (e.g., photoshare-alb-123.us-east-1.elb.amazonaws.com) from the ALB Description tab. You will need this URL to access your website and to configure the Lambda environment variables later.