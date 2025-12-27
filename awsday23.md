# ✅ AWS Day-23: Data Migration Between S3 Buckets (AWS CLI)

🔹 Goal
Create S3 buckets
Understand useful aws s3 commands
Migrate data from old bucket → new bucket
Verify and compare data after migration

🔹 Step 1: Check AWS S3 CLI Documentation
Before working, it’s good practice to review official docs:

👉 Google search: aws command line documentation s3
This helps you understand available commands like:
mb (make bucket)
ls (list)
sync (data migration)

🔹 Step 2: Create a New S3 Bucket
aws s3 mb s3://<bucket-name> --region us-east-1

Explanation:
mb → make bucket
s3://<bucket-name> → globally unique bucket name
--region us-east-1 → bucket region

📌 Example:
aws s3 mb s3://xfusion-new-bucket --region us-east-1

🔹 Step 3: List All S3 Buckets
aws s3 ls
✔ Shows all buckets in your AWS account
✔ Confirms bucket creation

🔹 Step 4: Explore S3 Help Commands
aws s3 help
aws s3 sync help

Explanation:
Displays available options and examples
Useful for learning flags like --delete, --dryrun, etc.

🔹 Step 5: Sync Data Between Two Buckets
aws s3 sync s3://<old-bucket-name> s3://<new-bucket-name>

Explanation:
Copies all objects from source to destination
Transfers only new or changed files
Preserves directory structure
📌 Example:
aws s3 sync s3://xfusion-old-bucket s3://xfusion-new-bucket

🔹 Step 6: Verify Migration (List Buckets)
aws s3 ls
✔ Confirms both buckets exist

🔹 Step 7: List Objects in Each Bucket
aws s3 ls s3://<old-bucket-name>
aws s3 ls s3://<new-bucket-name>

Explanation:
Compare object names and structure
Ensures data copied successfully

🔹 Step 8: Deep Verification with Recursive Listing
aws s3 ls s3://<bucket-name> --recursive --summarize

Explanation of Flags:
--recursive → lists all objects in all folders
--summarize → shows total count and size

Sample Output:
Total Objects: 349
Total Size: 76133582
✔ Run this command on both buckets
✔ Totals should match → migration successful

🔹 Data Comparison Strategy
Check	Old Bucket	New Bucket
Object count	✅	✅
Total size	✅	✅
Folder structure	✅	✅
If all match → Data migration completed successfully

# 📝 Summary
Created S3 bucket using CLI
Used aws s3 sync for migration
Verified data integrity using recursive listing
Compared object count and size

# 🎯 DevOps / Interview Notes
aws s3 sync is idempotent
Faster than manual copy
Commonly used for:
Backup
Disaster recovery
Cross-environment migration
Supports --dryrun for safe testing