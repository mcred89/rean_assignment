# Intro:

This CloudFormation template is designed to deploy 2 servers: A WordPress site run on docker containers and an Ansible node to keep the WordPress site configuration in a consistent state. 
It is configured such that the nodes will be fully configured and running at the end of the CloudFormation deploy. 

All of the scripts for this CloudFormation template can be found on: https://github.com/mcred89/rean_assignment

# Usage: 

There are a couple of assumtions made with this CloudFormation template:
1: The below command and associate scirpts assume you are in the REAN lab environment.
2: You have adequate permission within this environment to create IAM Roles.
3: You have access to the 'McIlroyKeyPair'.

Run the below command from an EC2 instance in the REAN Cloud lab environment:

aws cloudformation create-stack --stack-name mcilroy-stack --template-url https://s3.us-east-2.amazonaws.com/mcilroy-bucket/WordPressSiteFormation.json --parameters ParameterKey=KeyName,ParameterValue="McIlroyKeyPair" ParameterKey=InstanceType,ParameterValue="t2.micro" --capabilities CAPABILITY_IAM

# Outline of how the template works:

The template:
- Creates a VPC with a subnet, routing tables, and an Internet Gateway.
- Creates a WordPress/Docker node with an associated security group.
    - This node pulls basic WordPress and MySQL images from Docker Hub.
- Creates an Ansible node that manages the WordPress node, in a seperate secutiry group.
    - This node pulls its playbook from git hub and SSH keys from S3. It then runs a playbook that checks the state of the docker containers every minute. 

# To Do:

- Add ability to dynmically choose key pair. 
    - Use Fn::Sub on the key copy from the CM node using a new Parameters declaration that subs enite 'from' portion of copy (s3://mcilroy-bucket/McIlroyKeyPair.pem)
    - Add additional instructions to README for key pair creation, placement and command formatting
- Get rid of IAM Role addition?
    - Remove 2 role creation sections, along with line in CM node creation. 
    - Can I make this optional? 
- Lock down ssh on web node to only come from CM node?
    - Requires a 'depends on'?
- Add parameter for mysql root password?
    - Not that important...but having the password hard coded is bugging me. 


