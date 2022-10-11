# Operationalizing an AWS ML Project

## Files
```
1. train_and_deploy-solution.ipynb: this is a notebook that contains code for training and deploying a computer vision model on AWS
2. hpo.py: a Python script that train_and_deploy-solution.ipynb notebook can use as its "entry point."
3. train.py: Script for training the final model
4. infernce2.py: this is a Python file that use for deploying your trained model to an endpoint
5. ec2train1.py: this is a Python file that use for training a model on an EC2 instance
6. lambdafunction.py: this is a starter Python script that you can use for your Lambda function - remember, it will take a few adjustments before it functions correctly as a Lambda function
```

## Step 1. Training and deployment on Sagemaker

### Create a Sagemaker instance for model training and deployment.

![Instance](https://github.com/yuting1214/Udacity_Proj4/blob/main/plots/instance.png)

### Deploy Endpoint

![Endpoint](https://github.com/yuting1214/Udacity_Proj4/blob/main/plots/endpoint.png)

Create one noraml endpoint and one endpoint with autoscaling to compare the difference.

### Test Endpoint

![Test](https://github.com/yuting1214/Udacity_Proj4/blob/main/plots/new_endpoint_test.png)


## Step 2. EC2 Training

Choose an EC2 instance of ml.m5.xlarge since using the Accelerated Computing instances like ml.p3.2xlarge needs the quota approval from AWS.

Remember to activate AWS Deep Learning AMIs to use the prebuilt configuration.

![EC2](https://github.com/yuting1214/Udacity_Proj4/blob/main/plots/ec2.png)

## Step 3. Setting up a Lambda function

Set up policy.

![Policy](https://github.com/yuting1214/Udacity_Proj4/blob/main/plots/lambda_iam.png)

Deploy a lambda function

![Lambda](https://github.com/yuting1214/Udacity_Proj4/blob/main/plots/lambda_function_test.png)

## Step 4. Lambda function security

For conveneince, I attach the full access of service like AmazonSageMakerFullAccess, but doing this would cause security vulnerabilities.

Why is this dangerous?

Having an IAM policy that allows all possible permissions for all possible services is a direct violation of the security principle of  least privilege. If a malicious actor were to gain access to a user or role with such a policy, they would have unlimited access to everything in your AWS account, and then it’s game over. It’s a better practice to assign only the permissions necessary, and nothing more.

[Reference](https://www.fugue.co/blog/6-big-aws-iam-vulnerabilities-and-how-to-avoid-them)

## Step 5. Concurrency and auto-scaling

Why should we need to consider Concurrency and auto-scaling?

During the stage of project buildling, the transmitted traffic will be trivial, Since the users in this case would mainly be you and your team. Your team is probably much smaller than the group of users who will use the project after it's deployed.

After a project is deployed, it will probably experience much more traffic. Moreover, the traffic it experiences may be unexpected: you may have an unexpected volume of users, or users may access your project at unexpected times or in unexpected ways.

High traffic requires heavy loads on your computing resources, and this can lead to slower response times. The delay that a computing resource has when it responds to a request is called latency. The higher the latency, the worse the user experiences.

Therefore, Concurrency and Auto-scaling are two main solution in AWS to address the latency issues.


Comparison of Concurrency and Auto-Scaling.

Concurrency refers to the ability of a **Lambda function** to serve multiple requests simultaneously.

Types of Concurrency:
* Reserved Concurrency: Guarantee a maxium number of instances
* Provisioned Concurrency: Always-on instances ready to respond

Autoscaling allows your deployed endpoints to respond to multiple requests at the same time.

In our case, to address the issues of latency, we set up an auto-scaling for the endpoint. When the incoming requests exceeds the specified threshold, the auto-scaling would automatically increate the number of instances to maintain a smooth traffic for end users.


![auto](https://github.com/yuting1214/Udacity_Proj4/blob/main/plots/auto_scaling_config.png)
