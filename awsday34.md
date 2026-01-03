# AWS – Day 34: Create an AWS Lambda Function Using AWS CLI

The Nautilus DevOps team is continuing their journey into serverless architecture by creating another AWS Lambda function. Unlike the previous task, this function must be created using the AWS CLI, which is already configured on the aws-client host.

The goal is to create a simple Python-based Lambda function that returns a greeting message along with an HTTP success status code.
Requirements:
Python Script Name: lambda_function.py
Lambda Function Name: xfusion-lambda-cli

Runtime: Python
Response Body: Welcome to KKE AWS Labs!
Status Code: 200
IAM Role: lambda_execution_role
Deployment Package: function.zip
Region: us-east-1
Tool: AWS CLI

1️⃣ Create the Lambda Python Script
You first create a Python file that contains the Lambda handler function.
---
vi lambda_function.py

File Content:
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Welcome to KKE AWS Labs!'
    }

🔍 Explanation:
lambda_handler is the entry point AWS Lambda looks for
event contains input data
context contains runtime information

The function returns:
statusCode: 200 → successful execution
body → greeting message

2️⃣ Verify the Python Script
cat lambda_function.py

This confirms the code is correct before packaging.

3️⃣ Zip the Lambda Function Code
AWS Lambda requires code to be uploaded as a ZIP archive.

zip function.zip lambda_function.py

🔍 Explanation:
function.zip is the deployment package
AWS Lambda extracts and runs code from this zip file

4️⃣ Verify Files
ls
ls -ls

You should see:
lambda_function.py
function.zip
This confirms the zip file was created successfully.

5️⃣ Create the Lambda Function Using AWS CLI

aws lambda create-function \
    --function-name xfusion-lambda-cli \
    --runtime python3.14 \
    --role arn:aws:iam::122396751203:role/lambda_execution_role \
    --handler lambda_function.lambda_handler \
    --zip-file fileb://function.zip

🔍 Explanation of Parameters:
Option	Purpose
--function-name	Name of the Lambda function
--runtime	Python runtime environment
--role	IAM role allowing Lambda execution
--handler	File name + function name
--zip-file	Deployment package

⚠️ Important Note:
As of now, Python 3.14 is not officially supported by AWS Lambda. In real environments, use:
--runtime python3.9
(or python3.10 / python3.11 depending on availability).

6️⃣ Verify Lambda Function Creation

aws lambda get-function --function-name xfusion-lambda-cli

🔍 What this does:
Confirms the Lambda function exists
Displays configuration, runtime, and IAM role
Ensures the upload was successful

7️⃣ Invoke the Lambda Function

aws lambda invoke \
    --function-name xfusion-lambda-cli \
    output.json

🔍 Explanation:
This command executes the Lambda function
The response is written to output.json

8️⃣ View the Output

cat output.json

Expected Output:
"Welcome to KKE AWS Labs!"

This confirms:
Lambda executed successfully
Status code 200 was returned
Correct message was delivered

✅ Final Result Summary
After completing all steps:
✔ Lambda function xfusion-lambda-cli is created
✔ Function uses Python runtime
✔ IAM role lambda_execution_role is attached
✔ Code is deployed via AWS CLI
✔ Function returns:

{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}