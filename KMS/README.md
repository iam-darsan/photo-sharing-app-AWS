# KMS 

We are storing sensitive database passwords, and we need to make sure they are unreadable to anyone who shouldn't see them. KMS (Key Management Service) acts as our digital vault. AWS provides a managed key specifically for Secrets Manager. Later, we will use this key to “lock” our database credentials so that even if someone accessed the encrypted file, it would appear as scrambled gibberish without the key.

## Tasks:

### Step 1: Open the KMS Console

Go to the AWS Key Management Service console.

### Step 2: Locate the AWS-Managed Key

Find the managed key:

Alias: alias/aws/secretsmanager

### Step 3: Record the Key ARN

Select the key and copy its Key ARN.

### Step 4: Verify Key Status

On the key details page, confirm:

Key state: Enabled

## Note:
- Is the AWS managed key alias/aws/secretsmanager identified?
- Is the key state Enabled?