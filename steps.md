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
