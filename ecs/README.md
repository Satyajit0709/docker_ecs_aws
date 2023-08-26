## Deploying the ECS Cluster using CloudFormation

CloudFormation code is present in 

### 1 
create one directory , go inside the directory and then pull the git.
mkdir docker_ecs_aws
cd docker_ecs_aws

git init
git pull  https://github.com/Satyajit0709/Satyajit_Python.git

#### 2
If you have cloned this repository, navigate to its directory in your terminal or command prompt. If you have saved only the CloudFormation template, make sure you're in its directory.

```bash
cd path/to/your/directory
```

#### 3
Run the following command to deploy the ECS cluster. 

```bash
aws cloudformation create-stack --stack-name MyECSStack --template-body file://ecs-cluster.yaml
```
A Stack with the name *MyECSStack* will be created

#### 4
To view the status of your stack as AWS CloudFormation creates it, you can use:

```bash
aws cloudformation describe-stacks --stack-name MyECSStack
```
Wait for the `"StackStatus"` field to show `"CREATE_COMPLETE"`.

#### 5
After the stack creation completes, retrieve the outputs (the ECS cluster name and ARN) using:

```bash
aws cloudformation describe-stacks --stack-name MyECSStack --query 'Stacks[0].Outputs'
```

### 6
After you're done with the ECS cluster and want to delete it and associated resources, run:

```bash
aws cloudformation delete-stack --stack-name MyECSStack
```

## Setting Up ECS Task Definition under Fargate Mode with CloudFormation
### 1 
File name - ecs-fargate-task-definition.yaml 
This CloudFormation template defines an ECS Task Definition and a Fargate service.
Subnet id and Security groups need to be amended based on your profile.

### 2
Execute the following command to deploy the CloudFormation template

```bash
aws cloudformation create-stack --stack-name MyFargateTaskDefinition --template-body file://ecs-service-task-definition.yaml --capabilities CAPABILITY_NAMED_IAM
```

## ECS Scaling and Monitoring
### 1
File name - ecs-scaling-policies.yaml

### 2 
Update the CloudFormation Stack with Scaling Configurations.
```bash
aws cloudformation deploy \
  --template-file ecs-scaling-policies \
  --stack-name YourStackName \
  --capabilities CAPABILITY_NAMED_IAM \
  --parameter-overrides ECSClusterName=YourClusterName ECSServiceName=YourServiceName
```

Remember to replace `YourStackName`, `YourClusterName` and `YourServiceName` with respective value from your account.

### 3. 
After deploying the scaling configurations, you can monitor the ECS service's metrics with Amazon CloudWatch:

