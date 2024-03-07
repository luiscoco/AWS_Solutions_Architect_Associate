# AWS Solutions Architect Associate

https://explore.skillbuilder.aws/learn/lp/1044/Solutions%2520Architect%2520-%2520Knowledge%2520Badge%2520Readiness%2520Path

https://explore.skillbuilder.aws/learn/learning_plan/view/1046/solutions-architect-learning-plan-includes-labs

## 1. How to create VPC, Subnet, Internet Gateway, Route Table

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
