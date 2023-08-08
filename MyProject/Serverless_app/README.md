The project "Serverless App - CloudMed" involves creating a serverless application using AWS services. The application sends reminder messages via email and SMS.

It consists of several stages:

Stage 1: Set up Simple Email Service (SES) and Lambda

  •	Configure SES for sending emails.
  
  •	Verify sender and receiver email addresses.
  
  •	Create an IAM role for the Lambda function.
  
  •	Create a Lambda function that uses SES to send emails.

  
Stage 2: Create State Machine & API Gateway
  •	Set up a state machine using Amazon States Language (ASL).
  
  •	Define states for waiting and sending emails.
  
  •	Configure the state machine to use the email-sending Lambda.
  
  •	Create an API Gateway to interact with the front-end of the application.

  
Stage 3: Create the S3 Bucket

  •	Create an S3 bucket for hosting the client-side application.
  
  •	Enable static website hosting for the bucket.
  
  •	Upload front-end files including HTML, CSS, and JavaScript.

  
Stage 4: Testing the Application

  •	Access the S3-hosted application.
  
  •	Input time duration and message.
  
  •	Provide the customer's email address.
  
  •	Trigger the state machine to send an email.
  
  •	Monitor the state machine's execution and logs.

  
The application allows users to schedule and send reminder emails via a user-friendly web interface. It leverages AWS services such as SES, Lambda, Step Functions,
SNS, and API Gateway to create a seamless serverless experience.
StateFunction:
![image](https://github.com/iYUSHI-i/AWS_Projects/assets/92771062/2ca813e1-42ca-4574-9a60-a68611d7fbf8)


Success Screenshot.
![image](https://github.com/iYUSHI-i/AWS_Projects/assets/92771062/ec86bab3-3c6f-4623-ba90-6d19a28524fe)

Reference: https://github.com/MoRoble/AWS-Projects/tree/main/Serverless_app



