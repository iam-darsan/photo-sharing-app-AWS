# Project Architecture

This project builds a resilient web application using AWS best practices. It separates public-facing components from sensitive data using network isolation and automates tasks to ensure scalability.

## What This Architecture Does:

 - VPC (Network) - Custom network split into Public Subnets (ALB + EC2) and Private Subnets (Database). Data layer stays   isolated from internet.
 - IAM Roles - iam_role_ec2 and iam_role_lambda act as temporary security badges. No hardcoded API keys.
 - KMS (Encryption) - AWS-managed key encrypts secrets. Even if data is accessed, it appears as scrambled gibberish.
 - RDS (Database) - MySQL runs in private zone with Public Access disabled. Only EC2 can talk to it.
 - Secrets Manager - Database password stored securely and retrieved at runtime. Credentials never in source code.
 - S3 (Storage) - Image bucket with "Block All Public Access" enabled. Photos served through the app, not directly.
 - EC2 (Compute) - Web server in Public Subnet running Docker. The heart of the PhotoShare app.
 - Lambda (Serverless) - Triggers automatically on S3 upload to process image metadata in background.
 - ALB (Load Balancer) - Secure entry point routing internet traffic to EC2. Protects infrastructure from direct exposure.
 - CloudWatch (Monitoring) - Dashboard for CPU usage and Lambda errors. Alerts if something breaks.