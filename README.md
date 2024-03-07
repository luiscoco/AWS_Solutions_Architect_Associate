# AWS Solutions Architect Associate (AWS CLI Commands samples)

## 1. How to create VPC, Subnet, Internet Gateway, Route Table with AWS CLI

Creating an Amazon Virtual Private Cloud (VPC) using the AWS Command Line Interface (CLI) involves a series of steps

Below is a basic example of how you can create a VPC using the AWS CLI:

**Create a VPC**:

```
aws ec2 create-vpc --cidr-block 10.0.0.0/16
```
This command creates a VPC with the CIDR block 10.0.0.0/16. Replace this with the CIDR block of your choice

**Tag the VPC**:

```
aws ec2 create-tags --resources vpc-id --tags Key=Name,Value=myVPC
```

Replace vpc-id with the ID of the VPC created in the previous step and myVPC with the name you want to give to your VPC

**Create a subnet within the VPC**:

```
aws ec2 create-subnet --vpc-id vpc-id --cidr-block 10.0.1.0/24
```

This command creates a subnet with CIDR block 10.0.1.0/24 within the VPC. Replace vpc-id with the ID of your VPC

**Create an internet gateway (IGW) and attach it to the VPC**:

```
aws ec2 create-internet-gateway
```

This command creates an internet gateway

Take note of the internet gateway ID returned

**Attach the gateway to the VPC**:

```
aws ec2 attach-internet-gateway --vpc-id vpc-id --internet-gateway-id igw-id
```

Replace vpc-id with the ID of your VPC and igw-id with the ID of the internet gateway

**Create a route table for the VPC**:

```
aws ec2 create-route-table --vpc-id vpc-id
```

Replace vpc-id with the ID of your VPC

**Create a route in the route table to point to the internet gateway**:

```
aws ec2 create-route --route-table-id route-table-id --destination-cidr-block 0.0.0.0/0 --gateway-id igw-id
```

Replace route-table-id with the ID of the route table created in step 5 and igw-id with the ID of the internet gateway

**Associate the subnet with the route table**:

```
aws ec2 associate-route-table --subnet-id subnet-id --route-table-id route-table-id
```

Replace subnet-id with the ID of the subnet created in step 3 and route-table-id with the ID of the route table created in step 5

These are the basic steps to create a VPC using the AWS CLI 

You can further configure your VPC by adding additional subnets, security groups, network ACLs, etc., as per your requirements

Make sure you have the necessary permissions to execute these commands

## 2. How to create AWS EC2 with AWS CLI

To create an AWS EC2 instance using the AWS CLI commands and run the provided user data script, you can follow these steps:

**Create a security group**: 

This security group should allow inbound traffic on port 80 for HTTP access and any other necessary ports for your application

```
aws ec2 create-security-group --group-name WebAppSecurityGroup --description "Security group for web application"
```

**Authorize inbound traffic for HTTP (port 80)**:

```
aws ec2 authorize-security-group-ingress --group-name WebAppSecurityGroup --protocol tcp --port 80 --cidr 0.0.0.0/0
```

**Launch an EC2 instance**:

```
aws ec2 run-instances --image-id ami-xxxxxx --count 1 --instance-type t2.micro --key-name your-key-pair-name --security-groups WebAppSecurityGroup --user-data file://user-data-script.sh
```

Replace ami-xxxxxx with the appropriate Amazon Linux AMI ID for your region and your-key-pair-name with the name of your EC2 key pair

Amazon Linux 2023 AMI (ami-0b7282dd7deb48e78)

**Amazon Linux 2023 user data script**:

**user-data-script.sh**

```
#!/bin/bash -ex
wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/DEV-AWS-MO-GCNv2/FlaskApp.zip
unzip FlaskApp.zip
cd FlaskApp/
yum -y install python3-pip
pip install -r requirements.txt
yum -y install stress
export PHOTOS_BUCKET=${SUB_PHOTOS_BUCKET}
export AWS_DEFAULT_REGION=<INSERT REGION HERE>
export DYNAMO_MODE=on
FLASK_APP=application.py /usr/local/bin/flask run --host=0.0.0.0 --port=80
```

## 3. How to create AWS S3 bucket

To create an AWS S3 bucket using the AWS CLI commands for storing images, follow these steps:

**Create an S3 Bucket**:

```
aws s3api create-bucket --bucket your-bucket-name --region your-region
```

Replace your-bucket-name with your desired bucket name and your-region with the AWS region where you want to create the bucket.

**Enable Bucket Versioning** (optional but recommended for data protection):

```
aws s3api put-bucket-versioning --bucket your-bucket-name --versioning-configuration Status=Enabled
```

**Configure Bucket Policy** (optional but recommended for access control):

Here's a basic example of a bucket policy allowing read access to all objects in the bucket:

**bucket-policy.json**

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::your-bucket-name/*"
        }
    ]
}
```

You can set this policy using the following command:

```
aws s3api put-bucket-policy --bucket your-bucket-name --policy file://bucket-policy.json
```

Replace **bucket-policy.json** with the path to your JSON bucket policy file

Now, your S3 bucket is created and ready to store images

You can upload images using the AWS CLI or any other AWS SDKs and tools

Also we can specify in the Principal the Role who is granted to access the S3 bucket

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowS3ReadAccess",
            "Effect": "Allow",
            "Principal": {
	    	"AWS": "arn:aws::AccountNumber:role/S3DynamoDBFullAccessRole"
            },
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::your-bucket-name/*"
        }
    ]
}
```

