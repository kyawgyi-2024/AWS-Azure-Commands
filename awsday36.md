# AWS-Day 36: Load Balancing EC2 Instances with Application Load Balancer

# Q.
The Nautilus Development Team needs to set up a new EC2 instance and configure it to run a web server. This EC2 instance should be part of an Application Load Balancer (ALB) setup to ensure high availability and better traffic management. The task involves creating an EC2 instance, setting up an ALB, configuring a target group, and ensuring the web server is accessible via the ALB DNS.
Create a security group: Create a security group named xfusion-sg to open port 80 for the default security group (which will be attached to the ALB). Attach xfusion-sg security group to the EC2 instance.
Create an EC2 instance: Create an EC2 instance named xfusion-ec2. Use any available Ubuntu AMI to create this instance. Configure the instance to run a user data script during its launch.

# This script should:
Install the Nginx package.
Start the Nginx service.
Set up an Application Load Balancer: Set up an Application Load Balancer named xfusion-alb. Attach default security group to the same.
Create a target group: Create a target group named xfusion-tg.
Route traffic: The ALB should route traffic on port 80 to port 80 of the xfusion-ec2 instance.
Security group adjustments: Make appropriate changes in the default security group attached to the ALB if necessary. Eventually, the Nginx server running under xfusion-ec2 instance must be accessible using the ALB DNS.
--------------------------------------------------------------------------------------------------

# 1️⃣ Create Security Group: xfusion-sg
This SG will allow HTTP traffic and be attached to EC2.

Steps;
Go to EC2 → Security Groups → Create security group
Name: xfusion-sg
Description: Allow HTTP traffic
VPC: Same VPC where ALB & EC2 will be created
Inbound Rules;
Type	Protocol	Port	Source
HTTP	TCP	80	0.0.0.0/0
Outbound Rules
Allow All traffic (default)
✅ Create security group

# 2️⃣ Create EC2 Instance: xfusion-ec2
Steps;
Go to EC2 → Instances → Launch instance
Name: xfusion-ec2
AMI:
Choose Ubuntu Server 22.04 LTS (or any Ubuntu AMI)
Instance type: t2.micro (free tier friendly)
Key pair: Optional (not required for this task)
Network Settings;
VPC: Same VPC as ALB
Subnet: Any public subnet
Auto-assign public IP: Enabled
Security Group: Select xfusion-sg

# 🔹 User Data Script (IMPORTANT)
Paste this into Advanced details → User data:
#!/bin/bash
apt update -y
apt install nginx -y
systemctl enable nginx
systemctl start nginx
✅ Launch the instance

# 3️⃣ Create Target Group: xfusion-tg
Steps;
Go to EC2 → Target Groups → Create target group
Target type: Instances
Name: xfusion-tg
Protocol: HTTP
Port: 80
VPC: Same VPC
Health Check
Protocol: HTTP
Path: /
Register Targets;
Select xfusion-ec2
Port: 80
Click Include as pending
Click Create target group

# 4️⃣ Create Application Load Balancer: xfusion-alb
Steps;
Go to EC2 → Load Balancers → Create Load Balancer
Select Application Load Balancer
Basic Configuration
Name: xfusion-alb
Scheme: Internet-facing
IP address type: IPv4
Network Mapping
VPC: Same VPC
Select at least 2 public subnets

# 5️⃣ Security Group for ALB (Default SG)
Attach default security group to ALB.
Default SG Inbound Rule (Modify if needed)
Type	Protocol	Port	Source
HTTP	TCP	80	0.0.0.0/0
Outbound: Allow all
✅ Save rules

# 6️⃣ Listener & Routing Configuration
Listener;
Protocol: HTTP
Port: 80
Forward to:
Target group: xfusion-tg
✅ Create ALB

# 7️⃣ Verify Target Health
Open Target Groups → xfusion-tg
Go to Targets;
Status should be: Healthy
If unhealthy:
Check EC2 security group allows port 80
Confirm Nginx is running

# 8️⃣ Test Using ALB DNS
Go to Load Balancers → xfusion-alb
Copy DNS name, example:
xfusion-alb-123456.ap-south-1.elb.amazonaws.com
Open it in browser
✅ Expected Result
You should see:
Welcome to nginx!

# 🔐 Final Architecture Summary
Internet
   |
[ Application Load Balancer ]
   |
[ Target Group (xfusion-tg) ]
   |
[ EC2: xfusion-ec2 ]
   |
[ Nginx Web Server ]
====================================================================================================