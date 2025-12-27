# ✅ Azure Day-23: Automate VM Setup using Custom Data (User Data)

🔹 Goal
Use Azure CLI to create a VM
Automatically install & start Nginx at boot time
Open HTTP port (80)
Verify VM and NSG configuration

🔹 Step 1: Verify Azure Account & Resource Groups
az account show
# Shows current Azure subscription details

az group list -o table
# Lists all resource groups in table format

🔹 Step 2: Set Resource Group Variable
rg_name="resource_name"
# Example: rg_name="xfusion-rg"
✔ Using variables avoids repeating long names

🔹 Step 3: Create User Data Script (nginx.sh)
vim nginx.sh
---
Press -> i and add:
#!/bin/bash
apt update -y
apt install -y nginx
systemctl start nginx
systemctl enable nginx
---
Save & exit: Esc :wq → Enter

🔹 Step 4: Verify Script Content
cat nginx.sh
---
✔ This script will:
Update packages
Install nginx
Start nginx immediately
Enable nginx on boot
---

🔹 Step 5: Create Azure VM with Custom Data
az vm create \
  --resource-group $rg_name \
  --name datacenter-vm \
  --image Ubuntu2404 \
  --location eastus \
  --size Standard_B1s \
  --admin-username azureuser \
  --generate-ssh-keys \
  --custom-data nginx.sh \
  --os-disk-size-gb 30 \
  --storage-sku Standard_LRS

🔍 Important Options Explained
Option	Purpose
--custom-data nginx.sh	Runs script on first boot
Ubuntu2404	Ubuntu 24.04 LTS
Standard_B1s	Low-cost VM
--generate-ssh-keys	Enables SSH access
--location eastus	Azure region

✔ This is Azure’s version of cloud-init / user-data

🔹 Step 6: Verify Resources Created
az resource list --resource-group $rg_name -o table
# ✔ Confirms VM, NIC, NSG, IP, Disk creation

🔹 Step 7: Open HTTP Port (80)
az vm open-port \
  --resource-group $rg_name \
  --name datacenter-vm \
  --port 80
# ✔ Adds inbound rule to Network Security Group (NSG)

🔹 Step 8: Verify NSG Rules
az network nsg rule list \
  --resource-group $rg_name \
  --nsg-name datacenter-vmNSG \
  -o table
# ✔ Confirms port 80 (HTTP) is allowed

🔹 Step 9: Check VM Details
az vm show \
  --resource-group $rg_name \
  --name datacenter-vm \
  --show-details
# ✔ Look for: Public IP
              Power state
              Provisioning status

🔹 Step 10: Access Web Server
Open browser: http://<public-ip>
✔ Nginx default page confirms automation worked

# 📝 Summary
Created VM using Azure CLI
Automated nginx installation using custom-data
Opened HTTP port using CLI
Verified NSG, VM, and service status

# 🎯 DevOps & Interview Tips
Custom Data runs only at first boot
Azure Custom Data ≈ AWS EC2 User Data
Automation reduces manual configuration errors
Always verify NSG rules when services are unreachable
Use scripts for scalable infrastructure
