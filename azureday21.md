# ✅ Azure Day-21: Generate SSH Key and Connect to VM

🔹 Goal
Generate SSH key pair
Understand SSH key files
Use public key for secure VM login

🔹 Generate SSH Key Pair
ssh-keygen

You will be prompted:
Enter file in which to save the key:   → Press Enter (default location)
Enter passphrase (empty for no passphrase): → Press Enter
Enter same passphrase again: → Press Enter

✔ This creates a default SSH key pair without a passphrase

🔹 Verify SSH Key Files
cd ~/.ssh/                         # Navigate to SSH directory
ls                                 # List SSH files

Expected files: authorized_keys
                id_rsa
                id_rsa.pub
🔍 File Explanation
File	Purpose
id_rsa	Private key (DO NOT SHARE)
id_rsa.pub	Public key (copy to server)
authorized_keys	Allowed public keys

🔹 View and Copy Public Key
cat id_rsa.pub                     # Display public SSH key

➡️ Copy the entire line (starts with ssh-rsa or ssh-ed25519)

🔹 Return to Home Directory
cd ~                               # Go back to home directory

🔹 Login to Azure VM Using SSH
ssh azureuser@<public-ip>
Example: ssh azureuser@20.52.11.90
Type yes when prompted to confirm host authenticity
Login succeeds using SSH key authentication

# 📝 Summary
Generated SSH key pair using ssh-keygen
Identified private and public keys
Used SSH key to securely connect to Azure VM

# 🎯 Interview & DevOps Tips
SSH keys are more secure than passwords
Private key stays on your local machine
Public key goes to server (authorized_keys)
Default SSH location: ~/.ssh/
Azure VMs commonly use key-based authentication only
