# ✅ Azure Day-24: Secure SSH Access to Azure VM (Key-Based Login)

🔹 Goal
Generate SSH key pair
Use key-based authentication instead of passwords
Secure SSH access to Azure Virtual Machine

🔹 Step 1: Navigate to SSH Directory
cd ~/.ssh/                         # Go to SSH configuration directory
ls                                 # List existing SSH files
This directory stores SSH keys and authorized access files.

🔹 Step 2: Return to Home Directory
cd ../                             # Move back to home directory

🔹 Step 3: Generate New SSH Key Pair
ssh-keygen

Press Enter for all prompts:
Default key location
No passphrase (for lab simplicity)
✔ Creates:
id_rsa (private key)
id_rsa.pub (public key)

🔹 Step 4: Verify Generated Keys
cd ~/.ssh/                         # Enter SSH directory again
ls                                 # Confirm key files exist

Expected files:
id_rsa
id_rsa.pub
authorized_keys

🔹 Step 5: View and Copy Public Key
cat id_rsa.pub                     # Display public SSH key
➡️ Copy the entire public key
(this will be added to the VM for authentication)

🔹 Step 6: Return to Home Directory
cd ~                               # Back to home directory

🔹 Step 7: Connect Securely to Azure VM
ssh azureuser@<public-ip>
Example: ssh azureuser@20.52.11.90
Type yes when prompted (first-time connection)
Login succeeds using SSH key authentication

# 📝 Summary
Generated SSH key pair locally
Verified public/private keys
Used public key for secure VM access
Enabled passwordless SSH login

# 🎯 Security & Interview Tips
🔐 SSH keys are more secure than passwords
❌ Never share id_rsa (private key)
✔ Public keys go into authorized_keys
Azure VMs are secure by default with SSH keys

Recommended: Disable password login in production
