# Secret Manager 

A classic mistake in app development is saving the database password directly in the source code (e.g., password="admin123"). If that code leaks, the database is compromised.

AWS Secrets Manager fixes this. We store the password in a secure, central vault. When our PhotoSharing app starts up, it asks AWS: "Please give me the password." AWS checks the security badge (IAM Role) and hands it over securely. This keeps our code clean and our secrets safe.

## Tasks:

### Step 1: Store the Credentials

Go to the Secrets Manager Dashboard and click Store a new secret.

Secret type: Other type of secret
- Key/value pairs: Add rows for your DB credentials:
- Key: username | Value: admin
- Key: password | Value: (Paste your password here)
- Key: engine | Value: mysql
- Key: host | Value: (Paste your RDS Endpoint here)
- Key: port | Value: 3306
- Key: dbname | Value: (Paste your Initial database name here)
- Encryption key: aws/secretsmanager (Default)

Step 2: Name and Create

- Secret name: photoshare/db/credentials
- Description: Database credentials for PhotoSharing App
- Rotation: Leave disabled (default).
- Click Store.

## Note:
Is the secret photoshare/db/credentials created?
Is the secret encrypted using the aws/secretsmanager key?