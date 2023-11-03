Login to aws console
https://905392101331.signin.aws.amazon.com/console

Challenge 1:
1. SSH into your instance and install python
2. Open port 80
3. Add additional volume
4. Stop instance
5. Terminate instance


Challenge 2:
Goal: Provision Ubuntu VM using AWS CLI  <br />
Step 1: Install AWS CLI <br />
Step 2: Create aws keys <br>
Step 3: AWS configure <br>
Step 4: write AWS cli command to provision <br>


aws help <br />
aws ec2 help <br />
aws configure <br />
#Set us-west-2 <br />
aws ec2 create-key-pair --key-name arjun --query 'KeyMaterial' --output text > arjun.pem <br />
aws ec2 describe-key-pairs --key-name arjun <br />
aws ec2 delete-key-pair --key-name arjun <br />
Copy ami from console add below <br />
aws ec2 run-instances --image-id ami-017fecd1353bcc96e --instance-type t2.micro --key-name arjun <br />
aws ec2 describe-instances --instance-ids i-0aa83e127818bc20f <br />
aws ec2 create-tags --resources i-5203422c --tags Key=Name,Value=MyInstance <br />
aws ec2 terminate-instances --instance-ids i-5203422c <br />


AWS SDK
1. Install VS Code <br />
2. Install AWS CLI <br />
3. Install Python 3.8 or later <br />
4. Install Boto3: pip install boto3 <br />


AWS SDK IAM docs <br />
https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/iam.html

Create iam user
```
import boto3

# Initialize the IAM client
iam = boto3.client('iam')

# Specify the username for the new IAM user
new_user_name = 'arjun2'

# Create the IAM user
try:
    response = iam.create_user(UserName=new_user_name)
    print(f"IAM User '{new_user_name}' created successfully")
except Exception as e:
    print(f"Error creating IAM User: {str(e)}")

```

Challenge:
Provision/Create s3 bucket through AWS console < br />
Upload sample files using SDK

Challenge:
Add functionality to move object from standar to standard-IA after 30 days
Move to glacier after 365 days


CloudWatch

Chanllenge:
1. Create on ec2 instance < br />
2. Create few charts under metrics < br />
3. Explore EC2 default dashboard < br />
4. Create your own dashboard inlcude diff widgets < br />
5. Create one alarm when CPU/Network/Memory is high which sends notiifcation

Create Lambda function to calculate area, zip the code and upload and run it on aws console < br />

mkdir lamda-func1  < br />
#create a file called lambda_function.py and add this code which calculates area < br />
```
import json
import logging

logger = logging.getLogger()
logger.setLevel(logging.INFO)

def lambda_handler(event, context):
   
    # Get the length and width parameters from the event object. The
    # runtime converts the event object to a Python dictionary
    length=event['length']
    width=event['width']
   
    area = calculate_area(length, width)
    print(f"The area is {area}")
       
    logger.info(f"CloudWatch logs group: {context.log_group_name}")
   
    # return the calculated area as a JSON string
    data = {"area": area}
    return json.dumps(data)
   
def calculate_area(length, width):
    return length*width


```
cd .\lamda-func1\ < br />
mkdir package < br />
pip install --target ./package boto3 < br />
cd package < br />
 zip -r ../my_deployment_package.zip . < br />
cd .. < br />
zip my_deployment_package.zip lambda_function.py < br />
#Upload this zip to console and test by passing below values  < br />
{ "length": 6,   "width": 7}


Challenge:

Exercise: addition of 2 number, zip and create lambda with AWS CLI, 

** USE python environment <br />
example: python -m venv venv

mkdir lambda-func2
cd lambda-func2
#create a file called lambda_function.py and add this code which calculates area
```
def lambda_handler(event, context):
    num1 = event['num1']
    num2 = event['num2']
    result = num1 + num2
    return {
        'statusCode': 200,
        'body': f'Sum: {result}'
    }

```
cd lambda-func2
python -m venv venv
source venv/bin/activate  # On Windows, use 'venv\Scripts\activate'
pip install boto3
zip -r ../my_deployment_package.zip .
cd ..
zip my_deployment_package.zip lambda_function.py
Cd ..
aws lambda create-function --function-name MySampleLambda --runtime python3.8 --handler lambda_function.lambda_handler --role arn:aws:iam::905392101331:role/service-role/arjun-func-role-93dycf4c --zip-file fileb://lambda-func2.zip
Test {"num1": 2,
    "num2":2
}


Challange: Build lambda and integrate with s3 event<br />
https://docs.aws.amazon.com/lambda/latest/dg/with-s3-example.html 


