# rean_assignment

aws cloudformation create-stack --stack-name mcilroy-stack --template-url s3://mcilroy-bucket/WordPressSiteFormation.json --parameters ParameterKey=KeyName,ParameterValue="McIlroyKeyPair" ParameterKey=InstanceType,ParameterValue="t2.micro" --capabilities CAPABILITY_IAM
