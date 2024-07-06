# Serverless App - CloudMed
involves creating a serverless application using AWS services. The application sends reminder messages via email and SMS.

It consists of several stages:

## Stage 1: Set up Simple Email Service (SES) and Lambda

  •	Configure SES for sending emails.
  
  •	Verify sender and receiver email addresses.
  
  •	Create an IAM role for the Lambda function.
  
  •	Create a Lambda function that uses SES to send emails.
  Lambda Function to send emails:
  
        import boto3, os, json
        
        FROM_EMAIL_ADDRESS = 'REPLACE_ME'
        
        ses = boto3.client('ses')
        
          def lambda_handler(event, context):
            # Print event data to logs
            print("Received event: " + json.dumps(event))
            
            # Publish message directly to email, provided by EmailOnly or EmailPar TASK
            ses.send_email(
                Source=FROM_EMAIL_ADDRESS,
                Destination={'ToAddresses': [event['Input']['email']]},
                Message={
                    'Subject': {'Data': 'Medicine Time!'},
                    'Body': {'Text': {'Data': event['Input']['message']}}
                }
            )
            return 'Success!'


  
## Stage 2: Create State Machine & API Gateway
  •	Set up a state machine using Amazon States Language (ASL).
  
  •	Define states for waiting and sending emails.
  
  •	Configure the state machine to use the email-sending Lambda.

  API Lambda Function:

import boto3, json, os, decimal

SM_ARN = 'YOUR_STATEMACHINE_ARN'

sm = boto3.client('stepfunctions')

def lambda_handler(event, context):
    # Print event data to logs
    print("Received event: " + json.dumps(event))

    # Load data coming from API Gateway
    data = json.loads(event['body'])
    data['waitSeconds'] = int(data['waitSeconds'])
    
    # Sanity check that all of the parameters we need have come through from API gateway
    # Mixture of optional and mandatory ones
    checks = []
    checks.append('waitSeconds' in data)
    checks.append(type(data['waitSeconds']) == int)
    checks.append('message' in data)

    # if any checks fail, return an error to API Gateway to return to the client
    if False in checks:
        response = {
            "statusCode": 400,
            "headers": {"Access-Control-Allow-Origin": "*"},
            "body": json.dumps({"Status": "Success", "Reason": "Input failed validation"}, cls=DecimalEncoder)
        }
    # If none, start the state machine execution and inform the client of 2XX success :)
    else: 
        sm.start_execution(stateMachineArn=SM_ARN, input=json.dumps(data, cls=DecimalEncoder))
        response = {
            "statusCode": 200,
            "headers": {"Access-Control-Allow-Origin": "*"},
            "body": json.dumps({"Status": "Success"}, cls=DecimalEncoder)
        }
    return response

    # This is a workaround for: http://bugs.python.org/issue16535
    # Reference https://stackoverflow.com/questions/11942364/typeerror-integer-is-not-json-serializable-when-serializing-json-in-python
    # https://stackoverflow.com/questions/1960516/python-json-serialize-a-decimal-object
    # Credit goes to the group :)
    class DecimalEncoder(json.JSONEncoder):
        def default(self, obj):
            if isinstance(obj, decimal.Decimal):
                return int(obj)
            return super(DecimalEncoder, self).default(obj)

  
  •	Create an API Gateway to interact with the front-end of the application.

  
## Stage 3: Create the S3 Bucket

  •	Create an S3 bucket for hosting the client-side application.
  
  •	Enable static website hosting for the bucket.
  
  •	Upload front-end files including HTML, CSS, and JavaScript.

  
## Stage 4: Testing the Application

  •	Access the S3-hosted application.
  
  •	Input time duration and message.
  
  •	Provide the customer's email address.
  
  •	Trigger the state machine to send an email.
  
  •	Monitor the state machine's execution and logs.

  
The application allows users to schedule and send reminder emails via a user-friendly web interface. It leverages AWS services such as SES, Lambda, Step Functions,
SNS, and API Gateway to create a seamless serverless experience.
![image](https://github.com/iYUSHI-i/AWS_Projects/assets/92771062/1e75eee8-a80c-41bb-90ef-a762696eaeb3)


StateFunction:


![image](https://github.com/iYUSHI-i/AWS_Projects/assets/92771062/2ca813e1-42ca-4574-9a60-a68611d7fbf8)


![image](https://github.com/iYUSHI-i/AWS_Projects/assets/92771062/ec86bab3-3c6f-4623-ba90-6d19a28524fe)

Reference: https://github.com/MoRoble/AWS-Projects/tree/main/Serverless_app



