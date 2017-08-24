# Intro:

This CloudFormation template is designed to deploy 2 servers: A WordPress site run on docker containers and an Ansible node to keep the WordPress site configuration in a consistent state. 
It is configured such that the nodes will be fully configured and running at the end of the CloudFormation deploy. 

All of the scripts for this CloudFormation template can be found on: https://github.com/mcred89/rean_assignment

# Usage: 

1: You have to have adequate permission to create IAM Roles.

2: Copy the EC2 key pair that you will be using into an S3 bucket. The key's view permissions can (and should) be set to private. This key will be used to connect to both EC2 instances and from the Ansible node to the WordPress node.  

3: You can run this CloudFormation either directly from the AWS console or by copying the JSON file into your working directory and running the below command.

If running the command: Fill out the 'KeyName' (without .pem) and 'Bucket' name values. This template will only accept 't2.mirco' and 't2.small' for the 'InstanceType' Value. Copy the entire block below.  

aws cloudformation create-stack --stack-name mcilroy-stack1 --template-body file://WordPressSiteFormation.json --parameters \
ParameterKey=KeyName,ParameterValue="KEYPAIR" \
ParameterKey=InstanceType,ParameterValue="t2.micro" \
ParameterKey=Bucket,ParameterValue="BUCKET" \
--capabilities CAPABILITY_IAM
 

# Outline of how the template works:

The template:
- Creates a VPC with a subnet, routing tables, and an Internet Gateway.
- Creates a WordPress/Docker node with an associated security group.
    - This node pulls basic WordPress and MySQL images from Docker Hub.
- Creates an Ansible node that manages the WordPress node, in a seperate secutiry group.
    - This node pulls its playbook from git hub and SSH keys from S3. It then runs a playbook that checks the state of the docker containers every minute. 


