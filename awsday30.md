# AWS Day-30 – NAT Instance (Private EC2 → Internet)

Goal
• Enable internet access for private EC2
• Use NAT Instance (cost-saving)
• Allow private EC2 to upload file to S3

Architecture (Quick Understanding)
Private EC2  →  NAT Instance (Public Subnet)  →  Internet  →  S3

NAT Instance Configuration (Command Line Sheet)
Logged into xfusion-nat-instance (Amazon Linux 2)

sysctl net.ipv4.ip_forward
# Check if IP forwarding is enabled
# Default value = 0 (disabled)

sudo sysctl -w net.ipv4.ip_forward=1
# Enable IP forwarding temporarily
# Allows instance to forward traffic between interfaces

sysctl net.ipv4.ip_forward
# Verify IP forwarding is enabled (value should be 1)

sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
# Add NAT rule
# Translates private IPs to NAT instance public IP
# eth0 = internet-facing interface

sudo yum install -y iptables-services
# Install iptables service
# Required to save NAT rules permanently

sudo service iptables save
# Save iptables NAT rules
# Ensures rules persist after reboot

sudo systemctl enable --now iptables
# Enable and start iptables service

sudo systemctl status iptables
# Verify iptables service is running

sudo iptables -t nat -L -n -v
# List NAT table rules
# Confirm MASQUERADE rule exists

history
# Display executed commands

# AWS Console Configuration (Important but No CLI)

1) Public subnet:
   • Name: xfusion-pub-subnet
   • Auto-assign public IP: ENABLED

2) NAT Instance:
   • Name: xfusion-nat-instance
   • AMI: Amazon Linux 2
   • Source/Destination Check: DISABLED
   • Security Group:
       - Allow inbound from private subnet
       - Allow all outbound

3) Route Table (Private Subnet):
   • 0.0.0.0/0 → NAT Instance ID

Verification ✅
• Private EC2 has cron job
• File upload succeeds after NAT works
• File appears in S3 bucket:
  xfusion-nat-29173/xfusion-test.txt

Key Exam Points ⭐
• NAT Instance must be in PUBLIC subnet
• Disable Source/Destination Check
• Enable IP forwarding
• Configure iptables MASQUERADE
• Private subnet route → NAT Instance

| Feature      | NAT Instance           | NAT Gateway |
| ------------ | ---------------------- | ----------- |
| Cost         | Low                    | High        |
| Manual setup | Yes                    | No          |
| Scaling      | Manual                 | Automatic   |
| Use case     | Labs / Small workloads | Production  |

One-Line Interview Answer ;
A NAT instance allows private EC2 instances to access the internet by forwarding traffic through a public EC2 instance with IP forwarding and NAT rules enabled.