## Azure Day-13: Configure SSH Key Authentication (azureuser → root)

### 🔹 Goal
* Understand SSH key-based authentication
* Copy public key from local machine
* Configure SSH access for **azureuser** and **root**
* Enable **passwordless SSH login** using keys

## 🔹 Step 1: Verify SSH Keys on Local Machine
pwd                                # Show current directory
cd ~/.ssh/                          # Go to SSH key directory
ls                                  # List SSH key files
cd ..                               # Go back to home directory

> ✔ `.ssh` directory stores SSH private & public keys
---
## 🔹 Step 2: Test SSH Login as azureuser
ssh azureuser@<public-ip>           # Login to Azure VM
exit                                # Logout

## 🔹 Step 3: Copy Public SSH Key
cat ~/.ssh/id_rsa.pub               # Display public SSH key

➡️ **Copy the entire key** (starts with `ssh-rsa` or `ssh-ed25519`)
---

## 🔹 Step 4: Login Again as azureuser
ssh azureuser@<public-ip>           # Login to VM again

## 🔹 Step 5: Switch to Root User
sudo su                             # Switch to root user
cd ~                                # Go to root home directory
pwd                                 # Confirm location (/root)
ls -al                              # List files including hidden

## 🔹 Step 6: Configure SSH for Root User
cd .ssh/                            # Enter root SSH directory
ls                                  # Check authorized_keys exists
vim authorized_keys                 # Edit authorized_keys file
* Press `i` → Insert mode
* Paste copied **public SSH key**
* Press `Esc`
* Type `:wq` → Enter (save & exit)

## 🔹 Step 7: Re-login & Reconfigure (Cleanup & Correct Key)
exit                                # Exit root
exit                                # Exit azureuser
---
ssh root@<public-ip>                # Attempt direct root login
ssh azureuser@<public-ip>           # Login as azureuser again
sudo su                             # Switch to root
cd ~/.ssh/
ls

vim authorized_keys                 # Open key file
### 🔧 Editor Actions
* `dd` → Delete all existing keys
* `i` → Insert mode
* Paste correct public key
* `Esc :wq` → Save & exit
cat authorized_keys                 # Recheck key content

## 🔹 Step 8: Final Verification
exit                                # Exit root
exit                                # Exit azureuser

ssh root@<public-ip>                # Login directly as root using SSH key
✔ Successful login confirms SSH key-based authentication is working

## 📝 Summary
* SSH public key copied from local machine
* Added key to `authorized_keys`
* Enabled passwordless SSH login
* Configured secure root access

---
## 🎯 Interview & DevOps Tips
* **id_rsa** → private key (never share)
* **id_rsa.pub** → public key (safe to copy)
* **authorized_keys** → list of allowed keys
* SSH keys are **more secure than passwords**
* Root SSH access should be **restricted in production**

✔ Common Azure & Linux hardening task
