# LAMBDA
When a user uploads a new photo, we want to automatically calculate its size and dimensions without slowing down the main website. If the Web Server handled this task, large uploads could affect performance for other users.

We solve this using AWS Lambda. As soon as a photo is uploaded to S3, Lambda wakes up, processes the file in the background, and then shuts down. This keeps the PhotoSharing app fast and responsive.

## Tasks:

### Step 1: Create the Lambda Function

Go to the Lambda Dashboard and click Create function.

- Function name: photoshare-metadata-extractor
- Runtime: Python 3.14 (or latest Python version)
- Architecture: x86_64
- Change default execution role:
- Select Use an existing role
- Existing role: Select iam_role_lambda (created in the previous IAM section)
  
### Step 2: Network Configuration

- Advanced Settings -> Enable VPC: Leave Unchecked (No VPC).
- Reason: Our Lambda needs to reach S3 and the ALB (which are on the public internet). Since we do not have a NAT Gateway in our Private Subnet, putting the Lambda inside the VPC would cut off its internet access and cause it to fail.

### Step 3: Update Function Code

- In your terminal, run the following command to view the source code:
  
- cat lambda_handler.py
- Copy the entire output of the command.
  
- In the AWS Console, go to the Code tab and paste the code you just copied.
  
- Click Deploy to save changes.

### Step 4: Configure Environment Variables

The code relies on two variables to know where to look for files and where to send data.

- Go to Configuration tab > Environment variables.
- Click Edit.
- Add the following Key-Value pairs:
- Key: S3_BUCKET | Value: (Your bucket name, e.g., photoshare-assets-xxxx)
- Key: ALB_DNS | Value: (Your ALB DNS, e.g., photoshare-alb-xxx.elb.amazonaws.com)
- Click Save.

### Step 5: Add S3 Trigger

Configure S3 to wake up this Lambda function when a new photo is uploaded.

- Go to the Function overview section and click Add trigger.
- Select a source: S3.
- Bucket: Select your photoshare-assets-xxxx bucket.
- Event types: Select All object create events.
- Recursive invocation: Acknowledge the warning.
- Click Add.

## Verification:
- Is the function using the existing iam_role_lambda?
- Is the S3 trigger configured for ObjectCreated events?
- Is the Lambda function running without VPC attached (allowing S3 access)?