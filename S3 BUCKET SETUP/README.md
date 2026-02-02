# S3 Bucket Setup

S3 is where the actual image files will live. However, we don't want just anyone on the internet browsing through our bucket. To protect user privacy, we enforce a "Block All Public Access" policy. Even though the photos are for a web app, users won't download them directly from S3. Instead, they view them through our secure Web App, which checks if they are allowed to see the image first. This gives us total control over our data.

## Tasks:

### Step 1: Create S3 Bucket
 Go to the S3 Console and create a new S3 bucket.

- Bucket Name: photoshare-assets-<your-unique-suffix>(Copy this name, you will need it for the Lambda configuration later)
- Region: us-east-1
- Default Encryption: Enable (AES-256)
  
### Step 2: Enable Block All Public Access
Once the bucket is created, go to the bucket's Permissions tab. Block all public access is enabled by default, but verify that all four options are still enabled:

- Block public access to buckets and objects granted through new access control lists (ACLs)
- Block public access to buckets and objects granted through any access control lists (ACLs)
- Block public access to buckets and objects granted through new public bucket or access point policies
- Block public and cross-account access to buckets and objects through any public bucket or access point policies

### Step 3: Verify Bucket Privacy
Confirm that the bucket is completely private.

- No public ACLs are applied
- No public bucket policies are attached
- All Block Public Access settings are enabled

## Note:
- Is an S3 bucket with prefix photoshare-assets- created?
- Is "Block all public access" enabled for the bucket?