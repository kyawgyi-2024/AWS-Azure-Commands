# Day 37: Managing EC2 Access with S3 Role-based Permissions | 100 Days of Cloud (AWS)
# Q.
The Nautilus DevOps team needs to set up an application on an EC2 instance to interact with an S3 bucket for storing and retrieving data. To achieve this, the team must create a private S3 bucket, set appropriate IAM policies and roles, and test the application functionality.
Task:
1) EC2 Instance Setup:
An instance named datacenter-ec2 already exists.
The instance requires access to an S3 bucket.
2) Setup SSH Keys:
Create new SSH key pair (id_rsa and id_rsa.pub) on the aws-client host and add the public key to the root user's authorized keys on the EC2 instance.
3) Create a Private S3 Bucket:
Name the bucket datacenter-s3-25876.
Ensure the bucket is private.
4) Create an IAM Policy and Role:
Create an IAM policy allowing s3:PutObject, s3:ListBucket and s3:GetObject access to datacenter-s3-25876.
Create an IAM role named datacenter-role.
Attach the policy to the IAM role.
Attach this role to the datacenter-ec2 instance.
5) Test the Access:
SSH into the EC2 instance and try to upload a file to datacenter-s3-25876 bucket using following command:
---
aws s3 cp <your-file> s3://datacenter-s3-25876/
---
Now run following command to list the upload file:
---
aws s3 ls s3://datacenter-s3-25876/
---

# 1️⃣ EC2 Instance (Already Exists)
✅ Instance name: datacenter-ec2
No creation needed. We will attach an IAM role later.

# 2️⃣ Setup SSH Keys (aws-client ➜ EC2)
🔹 Create SSH key pair on aws-client
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa

This creates:
~/.ssh/id_rsa
~/.ssh/id_rsa.pub

# 🔹 Copy public key to EC2 root user
First, SSH to the EC2 instance (use correct key/IP if already exists):
ssh ec2-user@<EC2_PUBLIC_IP>

Switch to root:
sudo -i

Create SSH directory:
mkdir -p /root/.ssh
chmod 700 /root/.ssh

From aws-client, copy the public key content:
cat ~/.ssh/id_rsa.pub

Paste it into EC2:
vi /root/.ssh/authorized_keys
Paste key → save → exit

Set permissions:
chmod 600 /root/.ssh/authorized_keys

✅ SSH key setup complete

# 3️⃣ Create a Private S3 Bucket
aws s3api create-bucket \
  --bucket datacenter-s3-25876 \
  --region us-east-1
🔒 Block public access (important):

aws s3api put-public-access-block \
  --bucket datacenter-s3-25876 \
  --public-access-block-configuration \
  BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true
Verify:

aws s3 ls
4️⃣ Create IAM Policy & Role
🔹 Create IAM Policy
Create policy file:

# vi s3-policy.json
Paste:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::datacenter-s3-25876/*"
    },
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::datacenter-s3-25876"
    }
  ]
}
# Create policy:

aws iam create-policy \
  --policy-name datacenter-s3-policy \
  --policy-document file://s3-policy.json
# 🔹 Create IAM Role
Create trust policy:
vi trust-policy.json
Paste:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
Create role:

aws iam create-role \
  --role-name datacenter-role \
  --assume-role-policy-document file://trust-policy.json
Attach policy to role:

aws iam attach-role-policy \
  --role-name datacenter-role \
  --policy-arn arn:aws:iam::<ACCOUNT_ID>:policy/datacenter-s3-policy
🔹 Create Instance Profile & Attach Role
aws iam create-instance-profile \
  --instance-profile-name datacenter-profile
Add role to profile:

aws iam add-role-to-instance-profile \
  --instance-profile-name datacenter-profile \
  --role-name datacenter-role
Attach profile to EC2:

aws ec2 associate-iam-instance-profile \
  --instance-id <EC2_INSTANCE_ID> \
  --iam-instance-profile Name=datacenter-profile
Verify:

aws ec2 describe-instances \
  --instance-ids <EC2_INSTANCE_ID> \
  --query "Reservations[].Instances[].IamInstanceProfile"
# 5️⃣ Test S3 Access from EC2
🔹 SSH into EC2
ssh root@<EC2_PUBLIC_IP>
Create test file:

echo "DevOps S3 Test" > testfile.txt
Upload to S3:

aws s3 cp testfile.txt s3://datacenter-s3-25876/
List bucket contents:

aws s3 ls s3://datacenter-s3-25876/
✅ You should see testfile.txt

✅ Final Verification Checklist
✔ EC2 instance exists
✔ SSH key-based access configured
✔ Private S3 bucket created
✔ IAM policy + role created
✔ Role attached to EC2
✔ S3 upload & list successful
================================================================================================

root@ip-172-31-81-59:~# ls
cd  snap
root@ip-172-31-81-59:~# touch kk.txt
root@ip-172-31-81-59:~# ls
cd  kk.txt  snap
root@ip-172-31-81-59:~# aws s3 cp kk.txt s3://datacenter-s3-25876/
upload: ./kk.txt to s3://datacenter-s3-25876/kk.txt
root@ip-172-31-81-59:~# aws s3 ls s3://datacenter-s3-25876/
2026-01-05 10:57:53          0 kk.txt

root@ip-172-31-81-59:~#
    2  aws s3 ls s3://datacenter-s3-25876/
    3  ls
    4  cd .cd/
    5  ls
    6  touch kk.txt
    7  ls
    8  aws s3 cp kk.txt s3://datacenter-s3-25876/
    9  aws s3 ls s3://datacenter-s3-25876/
root@ip-172-31-81-59:~# 

✅ Great job — everything is working perfectly.