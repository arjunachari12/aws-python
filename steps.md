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

Serverless code samples
https://github.com/awsdocs/aws-doc-sdk-examples
https://docs.aws.amazon.com/lambda/latest/dg/service_code_examples_serverless_examples.html

Exercise: API Gateway<br />
Provision Lambda and integrate with API Gateway<br />
Create 2 routes, integrate to same lambda<br />
Create UAT stage<br />
Follow this to provision Lambda and API gateway <br />https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started.html<br />

Excercise: API Gateway, REST API <br />
https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-create-api-from-example.html  <br />

Exercise: Do CRUD with DynamoDB, use Console or AWS CLI or both <br />
https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStartedDynamoDB.html<br/>

Exercise: Do CRUD with DynamoDB using AWS SDK<br />
https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.html<br />

Exercise: Implement CRUD using DynamoDB, Lambda and API Gateway<br />
https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-dynamo-db.html#http-api-dynamo-db-create-function<br />

lambda function 

```
import json
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('http-crud-tutorial-items')

def lambda_handler(event, context):
    body = None
    status_code = 200
    headers = {
        "Content-Type": "application/json"
    }

    try:
        route_key = event['routeKey']
        if route_key == "DELETE /items/{id}":
            table.delete_item(
                Key={'id': event['pathParameters']['id']}
            )
            body = f"Deleted item {event['pathParameters']['id']}"
        elif route_key == "GET /items/{id}":
            response = table.get_item(
                Key={'id': event['pathParameters']['id']}
            )
            item = response.get('Item')
            if item is not None:
                body = item
            else:
                status_code = 404
                body = "Item not found"
        elif route_key == "GET /items":
            response = table.scan()
            items = response.get('Items')
            body = items
        elif route_key == "PUT /items":
            request_json = json.loads(event['body'])
            table.put_item(
                Item={
                    'id': request_json['id'],
                    'price': request_json['price'],
                    'name': request_json['name']
                }
            )
            body = f"Put item {request_json['id']}"
        else:
            raise Exception(f"Unsupported route: {route_key}")
    except Exception as e:
        status_code = 400
        body = str(e)
    finally:
        body = json.dumps(str(body))

    return {
        'statusCode': status_code,
        'body': body,
        'headers': headers
    }

```

Cloud Formation Teramplate<br />
```
Resources:
  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: "arjun2323bsdcdf"
```
https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-s3-bucket.html<br />

Challenge:<br />
Modify Cloudformation template to read bucket name as parameter and run in on console<br />
```
Parameters:
  S3bucketName:
    Type: String 
    Description: "Bucket Name"
Resources:
  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Ref S3bucketName
Outputs:
  BucketName:
    Value: !Ref MyS3Bucket
```
you can post your questions here<br />

Install Terraform<br />
https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli<br />
https://docs.google.com/document/d/1CwJaWuKZuFoSfl8SJmgJcj8ZZ10d6Aq-8eYgrsI65fU/edit?usp=sharing<br />

Terraform Providers<br />
https://registry.terraform.io/browse/providers<br />
https://registry.terraform.io/providers/hashicorp/aws/latest/docs<br />

