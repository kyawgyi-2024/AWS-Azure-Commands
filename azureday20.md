# ✅ Azure Day-20: Download File from Azure Blob Storage

🔹 Goal
Download a file from Azure Blob Storage
Save it locally on the VM
Verify file content

🔹 Navigate to Working Directory
cd /opt/                         # Move to /opt directory (target location)
cd ../                           # Move one level up (optional navigation check)

✔ /opt is commonly used for application or shared files

🔹 Azure Blob Download Command (Syntax Reference)
az storage blob download \
  -f /path/to/file \
  -c <container-name> \
  -n <blob-name>

🔍 Option Explanation
Option	Meaning
-f	Local file path to save the blob
-c	Blob container name
-n	Blob (object) name

🔹 Download Blob from Azure Storage Account
az storage blob download \
  -f /opt/xfusion.txt \
  -c xfusion-blob-1234 \
  -n xfusion.txt \
  --account-name xfusionst31145

🔍 What this does
Downloads xfusion.txt from Azure Blob container
Saves it locally at /opt/xfusion.txt
Uses storage account xfusionst31145

ℹ️ Authentication is done via:
Azure login session OR
Storage account key / SAS (pre-configured)

🔹 Verify Downloaded File
cd /opt/                         # Go to destination directory
ls                               # Confirm xfusion.txt exists
cat xfusion.txt                  # Display file content

✔ Successful output confirms:
Azure CLI access is working
Blob download completed successfully

# 📝 Summary
Used Azure CLI to download blob storage file
Saved file to /opt
Verified content locally

# 🎯 Interview Tips
Azure Blob Storage ≈ AWS S3
az storage blob download → object download
Containers ≈ buckets
Always verify file permissions and content
Azure CLI must be authenticated (az login)
