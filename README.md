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

## Step 5. Concurrency and auto-scaling

Set up an endpoint with auto-scaling.

![auto](https://github.com/yuting1214/Udacity_Proj4/blob/main/plots/auto_scaling_config.png)
