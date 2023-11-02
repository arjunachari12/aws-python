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
#Set us-west-2
aws ec2 create-key-pair --key-name arjun --query 'KeyMaterial' --output text > arjun.pem
aws ec2 describe-key-pairs --key-name arjun
aws ec2 delete-key-pair --key-name arjun
Copy ami from console add below
aws ec2 run-instances --image-id ami-017fecd1353bcc96e --instance-type t2.micro --key-name arjun
aws ec2 describe-instances --instance-ids i-0aa83e127818bc20f
aws ec2 create-tags --resources i-5203422c --tags Key=Name,Value=MyInstance
aws ec2 terminate-instances --instance-ids i-5203422c



