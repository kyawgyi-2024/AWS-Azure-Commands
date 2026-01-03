# ✅ Day 35: Deploying and Managing Applications on AWS

🔹 Rewritten Task Description (Simplified & Clear)
The Nautilus DevOps team requires a private Amazon RDS MySQL database for their application. The database must be securely accessible only from an existing EC2 instance. Once the database is ready, the EC2 instance should connect to it using a PHP application and confirm connectivity through a web browser.

🔹 Part 1: Create a Private RDS MySQL Instance
📌 RDS Requirements
DB Identifier: devops-rds
Engine: MySQL 8.4.5
Instance Class: db.t3.micro
Master Username: devops_admin
Storage Type: gp2
Storage Size: 5 GiB
Initial Database Name: devops_db
Public Access: ❌ No (Private RDS)
State: Available
Other Settings: Keep defaults (sandbox template)

🔍 Why these settings?
Private RDS → Improves security
db.t3.micro → Cost-effective for testing
gp2 storage → General-purpose SSD
MySQL 8.4.5 → Modern and stable engine

🔹 Security Group Configuration
📌 Required Rules
Inbound rule for RDS
Port: 3306
Source: Security group of devops-ec2
Inbound rule for EC2
Port: 80
Source: 0.0.0.0/0

🔍 Why?
Port 3306 → MySQL database access
Port 80 → Web access to PHP app
Restricting RDS access to EC2 → Secure architecture

🔹 Part 2: Configure Password-less SSH Access
📌 Objective
Allow the aws-client host to SSH into devops-ec2 without a password.

🔹 Steps Performed
1️⃣ Navigate to SSH Directory
---
cd ~/.ssh

2️⃣ Generate SSH Key (if not exists)
---
ssh-keygen

This creates:
id_rsa → private key
id_rsa.pub → public key

3️⃣ View Public Key
---
cat id_rsa.pub

4️⃣ Add Public Key to EC2
The public key is added to:
/root/.ssh/authorized_keys

on the devops-ec2 instance.
🔍 Result
You can now SSH into EC2 without entering a password.

🔹 Part 3: Copy PHP Application to EC2
📌 Source File
Located on aws-client:
/root/index.php

📌 Destination on EC2
/var/www/html/index.php
This directory is the Apache web root.

🔹 PHP File Configuration (RDS Connection)
<?php
$dbname = 'devops_db';
$dbuser = 'devops_admin';
$dbpass = 'ZBo2eqfGO4SKuNuqTzlC';
$dbhost = 'devops-rds.cdiv6lyuucez.us-east-1.rds.amazonaws.com';

$link = mysqli_connect($dbhost, $dbuser, $dbpass)
  or die("Unable to Connect to '$dbhost'");
mysqli_select_db($link, $dbname)
  or die("Could not open the db '$dbname'");

$test_query = "SHOW TABLES FROM $dbname";
$result = mysqli_query($link, $test_query);

$tblCnt = 0;
while ($tbl = mysqli_fetch_array($result)) {
  $tblCnt++;
}

if (!$tblCnt) {
  echo "Connected successfully<br />\n";
}
?>

🔍 What this script does:
Connects to the RDS MySQL instance
Selects the database
Runs a test query
Prints “Connected successfully” if connection works

🔹 Part 4: Web Server Setup on EC2
📌 Apache Web Root
---
cd /var/www/html/
Remove default page
---
rm index.html

Ensure PHP file is present
---
ls

Test locally
---
curl localhost

✅ Output:
Connected successfully

🔹 Part 5: Verify from Browser
Open browser
Enter EC2 Public IP
You should see:
Connected successfully

✅ Final Outcome Summary
✔ Private RDS MySQL instance created
✔ Secure EC2 ↔ RDS connectivity configured
✔ Password-less SSH enabled
✔ PHP app successfully connects to RDS
✔ Application accessible via browser

🎯 Key AWS Concepts Learned
Amazon RDS (private DB)
Security Groups
EC2 ↔ RDS connectivity
SSH key-based authentication
PHP–MySQL integration
Real-world 3-tier architecture basics