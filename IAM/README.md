# IAM ROLES

Our PhotoSharing app has two main workers: the Web Server (EC2) and the Image Processing Function (Lambda).

Instead of hardcoding dangerous API keys, we create specific IAM Roles. These roles act as temporary ID badges, granting only the necessary permissions. Crucially, we must use specific role names starting with iam and containing role to comply with the lab's security policy.

## Tasks:

### Step 1: Create the EC2 Role

Go to the IAM Dashboard, select Roles, and click Create role.

- Trusted entity type: AWS service
- Service or use case: EC2
- Add permissions: Search for and select these policies:
- AmazonS3FullAccess
- AWSSecretsManagerClientReadOnlyAccess
- Role name: iam_role_ec2

### Step 2: Create the Lambda Role

Click Create role again to set up the role for your serverless function.

- Trusted entity type: AWS service
- Service or use case: Lambda
- Add permissions: Search for and select these policies:
- AWSLambdaBasicExecutionRole
- AmazonS3FullAccess
- Role name: iam_role_lambda

Note:
Are IAM Roles iam_role_ec2 and iam_role_lambda created?