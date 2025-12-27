# ✅ Azure Day-3: Create Virtual Machine using Azure CLI

🔹 Goal
Verify Azure account access
Identify Resource Group
Create an Ubuntu 22.04 VM
Retrieve public IP
SSH into the VM

🔹 Check Azure Account & Resource Groups
az account show
# Shows current Azure subscription details

az group list
# Lists all available Resource Groups in the subscription

🔹 Set Resource Group Variable
RG_NAME="your-resource-group-name"
# Example:
# RG_NAME="xfusion-rg"
✔ Using variables avoids typing the resource group name repeatedly

🔹 Create Azure Virtual Machine
az vm create \
  --resource-group $RG_NAME \
  --name xfusion-vm \
  --image ubuntu22.04 \
  --size Standard_B2s \
  --admin-username azureuser \
  --generate-ssh-keys \
  --storage-sku Standard_LRS \
  --os-disk-size-gb 30

🔍 Explanation of Important Options
Option	Meaning
--resource-group	Resource group to place VM
--name	Virtual machine name
--image ubuntu22.04	OS image (Ubuntu 22.04 LTS)
--size Standard_B2s	VM size (2 vCPU, 4GB RAM)
--admin-username	Linux login user
--generate-ssh-keys	Creates SSH key if missing
--storage-sku Standard_LRS	Standard managed disk
--os-disk-size-gb 30	OS disk size (30 GB)

🔹 Verify VM Details
az vm show \
  --resource-group $RG_NAME \
  --name xfusion-vm
# Displays VM configuration (JSON output)

🔹 Get Public IP Address
az vm show \
  --resource-group $RG_NAME \
  --name xfusion-vm \
  --show-details \
  --query "publicIps" \
  --output tsv

✔ --query filters only the public IP
✔ --output tsv removes quotes

🔹 Connect to VM via SSH
ssh azureuser@<public-ip>
Example: ssh azureuser@20.52.11.90

✔ Successful login confirms:
VM is running
Network + SSH access works

# 📝 Summary
Verified Azure subscription and resource groups
Created Ubuntu VM using Azure CLI
Retrieved public IP via CLI query
Connected securely using SSH key authentication

# 🎯 Interview Tips (Important)
Azure VM ≈ AWS EC2
Resource Group ≈ Logical container
Standard_B2s → cost-effective dev VM
SSH keys are more secure than passwords
Azure auto-creates:
NIC
Public IP
OS disk
