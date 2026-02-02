# Cloud Watch

Since our Lambda function runs in the background to process photos, we wouldn't see it failing just by looking at the website. We need a way to monitor the "invisible" parts of our app. CloudWatch is our dashboard. We set up a monitor to track Lambda errors. If the image processing breaks, CloudWatch will alert us immediately, ensuring we can fix the issue before users start complaining that their photo details are missing.

## Tasks:

### Step 1: Create the Dashboard

Go to the CloudWatch Console and select Dashboards from the left menu.

- Click: Create dashboard
- Name: PhotoShare-Monitor
- Click: Create dashboard

### Step 2: Add EC2 CPU Widget

You will be prompted to add a widget immediately after creating the dashboard.

- Widget Type: Line
- Metrics: EC2 > Per-Instance Metrics > CPUUtilization
- Instance: Select your photoshare-web instance
- Click: Create widget

### Step 3: Add Lambda Invocations Widget

Add another widget to track how often your function runs.

- Click: Add widget
- Widget Type: Number
- Metrics: Lambda > By Function Name > Invocations
- Function: Select your photoshare-metadata-extractor
- Click: Create widget and Save dashboard.

### Step 4: Create Alarm for Lambda Errors

Set up an alert so you know if the code fails.

- Go to Alarms -> All alarms -> Create alarm.
- Select metric: Lambda > By Function Name > Errors (Select your function).
- Conditions:
- Threshold type: Static
- Whenever Errors is…: Greater than 0
- Name: PhotoShare-Lambda-Error-Alarm
- Click: Create alarm.

## Verification:
- Is the PhotoShare-Monitor dashboard showing data for both EC2 and Lambda?
- Is the PhotoShare-Lambda-Error-Alarm state currently OK (green)?
- Action Required: Verify the alarm exists via CLI: aws cloudwatch describe-alarms --alarm-names PhotoShare-Lambda-Error-Alarm