# AWS – Day 33: Creating an AWS Lambda Function

The Nautilus DevOps team is moving toward a serverless architecture and wants to demonstrate how AWS Lambda works. To do this, they will create a simple Lambda function that returns a custom greeting message.
Your task is to create an AWS Lambda function using the AWS Management Console with the following requirements:

Lambda Function Name: xfusion-lambda
Runtime: Python
IAM Role: lambda_execution_role
Response Body: Welcome to KKE AWS Labs!
HTTP Status Code: 200

This Lambda function will help demonstrate how quickly applications can be deployed and scaled using AWS Lambda.

🔹 Explanation of Each Requirement
1️⃣ AWS Lambda Function
AWS Lambda is a serverless compute service that lets you run code without managing servers. You only upload your code, and AWS handles everything else like scaling and availability.

2️⃣ Function Name: xfusion-lambda
This is the unique name of your Lambda function.
It helps identify the function inside your AWS account.

3️⃣ Runtime: Python
The runtime defines the programming language used by your Lambda function.
You must select Python (e.g., Python 3.9 or 3.10) when creating the function.

4️⃣ IAM Role: lambda_execution_role
Lambda functions need permission to interact with AWS services.
The IAM role:
Allows Lambda to write logs to CloudWatch
Grants execution permissions
You must create and attach a role named lambda_execution_role.

5️⃣ Function Output (Response Body)
The function should return the following message:
Welcome to KKE AWS Labs!
This message will be sent back as the response body when the Lambda function is executed.

6️⃣ Status Code: 200
A status code of 200 means the request was successful.
This is commonly used in APIs to indicate a successful response.

🔹 Example Lambda Function Code (Python)
When writing the function code in the AWS Console, it should look like this:
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Welcome to KKE AWS Labs!'
    }

🔍 What this code does:
lambda_handler is the entry point for the Lambda function
statusCode: 200 indicates success
body contains the greeting message

🔹 Final Outcome
After completing this task:
The Lambda function xfusion-lambda will be created
It will run using Python
It will use the lambda_execution_role
On execution, it will return:
Status Code: 200
Message: Welcome to KKE AWS Labs!