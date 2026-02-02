# APP VALIDATION 

This is the moment of truth. We are going to use the app just like a real user to prove that everything is connected correctly. When you open the App via the Load Balancer and upload a photo, you are testing the entire chain: The network routing, the database login, the S3 storage, and the Lambda trigger. If the image uploads and the dashboard updates, you have successfully built a secure, production-ready cloud architecture!

## Tasks:

### Step 1: Get Load Balancer DNS

You need the public address of your application.

- Navigate to the EC2 Console -> Load Balancers.
- Select photoshare-alb.
- Locate the DNS name in the Details tab and copy it (e.g., photoshare-alb-12345.us-east-1.elb.amazonaws.com).

### Step 2: Access & Upload

- Open a new browser tab and paste the DNS Name.
- Verify the PhotoShare application loads successfully.
- Click the Share Your Photo button and select a sample image from your computer.

### Step 3: Verify Backend Processing

Confirm that the invisible parts of the infrastructure worked.

- Go to the S3 Console -> Open your bucket (photoshare-assets-...).
- Verify the image file appears there.
- Go to the CloudWatch Console -> Dashboards -> PhotoShare-Monitor.
- Check if the Lambda Invocations widget shows a data point (it might take a minute).

##Verification:
- Did the web page load using the ALB DNS?
- Did the image upload successfully to S3?
- Action Required: If you cannot access the browser, verify connectivity via terminal:
- curl -I http://<ALB-DNS-NAME>