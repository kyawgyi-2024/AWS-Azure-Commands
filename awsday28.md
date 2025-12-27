# ✅ AWS Day-28: Create a Private ECR Repository & Push Docker Image

🔹 Goal
Build a Docker image
Create a private Amazon ECR repository
Authenticate Docker with ECR
Push the image to ECR (us-east-1)

🔹 Environment Info
Region: us-east-1
Project directory: /root/pyapp
AWS Account ID: 187354226259
ECR Repository Name: datacenter-ecr

🔹 Step 1: Navigate to Application Directory
cd /root/pyapp
ls

Explanation:
Move into the project folder containing Dockerfile
Verify application files exist

🔹 Step 2: Check Existing Docker Images
docker images

Explanation:
Lists all Docker images on the system
Confirms no existing pyapp image before build

🔹 Step 3: Build Docker Image
docker build -t pyapp .

Explanation:
docker build → builds image from Dockerfile
-t pyapp → tags image with name pyapp
. → uses current directory
✔ Result: Docker image named pyapp

🔹 Step 4: Verify Image Creation
docker images
✔ Confirms pyapp image is created successfully

🔹 Step 5: Tag Image for Amazon ECR
docker tag pyapp 187354226259.dkr.ecr.us-east-1.amazonaws.com/datacenter-ecr

Explanation:
ECR requires full repository URI
Format:
<aws_account_id>.dkr.ecr.<region>.amazonaws.com/<repository_name>
✔ This prepares the image for pushing to ECR

🔹 Step 6: Authenticate Docker to ECR
aws ecr get-login-password --region us-east-1 \
| docker login --username AWS --password-stdin 187354226259.dkr.ecr.us-east-1.amazonaws.com

Explanation:
aws ecr get-login-password → generates temporary auth token
docker login → authenticates Docker with ECR
Uses IAM permissions, not static passwords
✔ Required before pushing images

🔹 Step 7: Push Image to Private ECR Repository
docker push 187354226259.dkr.ecr.us-east-1.amazonaws.com/datacenter-ecr

Explanation:
Uploads Docker image layers to ECR
Stores image privately inside AWS account
✔ Image is now available in Amazon ECR

🔹 Generic ECR Login Command (Template)
aws ecr get-login-password --region <region> \
| docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com

Example:
aws ecr get-login-password --region us-east-1 \
| docker login --username AWS --password-stdin 187354226259.dkr.ecr.us-east-1.amazonaws.com

🔹 Verify in AWS Console
Go to Amazon ECR
Open datacenter-ecr
Check Images tab
Image should be visible 🎉

# 📝 Summary
✔ Built Docker image
✔ Tagged image for ECR
✔ Authenticated Docker with AWS
✔ Pushed image to private ECR repository

# 🎯 DevOps / Interview Tips
ECR is private by default
Authentication token expires every 12 hours
Used heavily with:
ECS
EKS
CI/CD pipelines
Safer than Docker Hub for production