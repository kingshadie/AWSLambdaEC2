# AWSLambdaEC2
This Repo provides an automation setup to stop and start an Amazon EC2 instance at scheduled times using AWS Lambda, SNS for notifications, and CloudWatch Events for scheduling.

**EC2 Instance Automation with AWS Lambda**

This project provides an automation setup to stop and start an Amazon EC2 instance at scheduled times using AWS Lambda, SNS for notifications, and CloudWatch Events for scheduling. The Lambda function stops the instance at 1 AM and restarts it at 7:30 AM daily, sending SNS notifications on each action.

**Features**

 - Automatically stops the EC2 instance at 1 AM and restarts it at 7:30 AM.

 - Sends notifications via SNS on instance shutdown and restart.

 - CloudWatch Events are used to schedule the actions.

 - Easy setup and configuration with environment variables.

**Prerequisites**

To set up this automation, you need the following:

 - AWS account with appropriate access permissions.

 - An EC2 instance running in your AWS environment.

 - An email address to receive SNS notifications.

**Steps to Setup**

**Step 1:** **Create the IAM Role for Lambda**

1. Go to the **IAM** service in the AWS Management Console.

2. Create a new role:

 - Trusted Entity Type: AWS Service

 - Use Case: Lambda

3. Attach the following policies:

 - AmazonEC2FullAccess
  
 - AmazonSNSFullAccess
  
4. Name the role something meaningful, e.g., EC2ControlLambdaRole.
   
5. Review and create the role.

**Step 2: Create the Lambda Function**

1. Go to the Lambda service in the AWS Console.

2. Create a new function:

 - Function name: ManageEC2Instance

 - Runtime: Python 3.9

 - Permissions: Use the IAM role created in Step 1.

3. Write the Lambda function code (available in the Lambda Function Code section).
   
4. Set environment variables for your Lambda function:
   
 - INSTANCE_ID: Your EC2 instance ID.
  
 - SNS_TOPIC_ARN: ARN of the SNS topic you’ll create for notifications.
  
**Step 3: Create SNS Topic and Subscription**

1. Go to the SNS service in the AWS Console.
   
2. Create a new topic:
   
 - Topic name: EC2InstanceNotifications
  
3. Create a subscription:
   
 - Protocol: Email
  
 - Endpoint: Your email address.
  
4. Confirm your subscription via the verification email.
   
**Step 4: Set Up CloudWatch Event Rules for Scheduling**

1. Go to the CloudWatch service in the AWS Console.
   
2. Create a rule to stop the EC2 instance at 1 AM:
   
 - Event Source: Schedule

 - Cron expression: cron(0 1 * * ? *)

 - Target: Your Lambda function.

 - Configure input: {"action": "stop"}

3. Create a rule to restart the EC2 instance at 7:30 AM:
   
 - Cron expression: cron(30 7 * * ? *)
  
 - Configure input: {"action": "start"}
  
**Step 5: Test the Lambda Function**

1. Manually trigger the Lambda function using the following test events:
   
 - For stopping: {"action": "stop"}
  
 - For starting: {"action": "start"}
  
**Step 6: Monitor and Optimize**

- Monitor Lambda execution logs in CloudWatch Logs.
  
- Ensure that SNS notifications are being received.
  
- Optimize EC2 uptime to avoid unnecessary charges.
  

  **Lambda Funcition Code**
  
![image](https://github.com/user-attachments/assets/84bb2fc6-a6e6-4981-b6e3-c3e07c7df922)

**Environment Variables**

- INSTANCE_ID: The ID of your EC2 instance.
  
- SNS_TOPIC_ARN: The ARN of your SNS topic to send notifications.
  
**Cron Expressions**

- Stop EC2 at 1 AM: cron(0 1 * * ? *)

- Start EC2 at 7:30 AM: cron(30 7 * * ? *)
