AWS Multi-Tier CloudFormation Template
This repository contains an AWS CloudFormation template that sets up a multi-tier architecture on AWS using VPC, subnets, Internet Gateway, NAT Gateway, and more. The architecture is distributed across multiple Availability Zones to provide high availability and fault tolerance.

Table of Contents
Architecture Overview
Template Features
Requirements
Resources Created
How to Deploy
Customization
Outputs
Architecture Overview
The CloudFormation template implements a multi-tier architecture using three distinct tiers:

Web Tier (Public Subnets): Public-facing layer for services like web servers.
App Tier (Private Subnets): A layer for internal application services that do not require direct public access.
DB Tier (Private Subnets): A secure layer for databases that are isolated from direct internet access.
The architecture spans across multiple Availability Zones (AZs) to ensure high availability and redundancy.

Template Features
VPC: A Virtual Private Cloud with CIDR 10.16.0.0/16.
Public Subnets: Subnets for web tier services in each Availability Zone.
Private Subnets: Subnets for application and database tiers in each Availability Zone.
Internet Gateway (IGW): Allows instances in public subnets to access the internet.
NAT Gateway: Enables instances in private subnets to access the internet (for software updates, etc.).
Route Tables: Separate route tables for public and private subnets.
Requirements
An AWS account with appropriate permissions to deploy CloudFormation stacks.
AWS CLI or access to the AWS Management Console.
IAM user with permissions for VPC, EC2, and CloudFormation services.
Resources Created
This CloudFormation template creates the following resources:

VPC: A Virtual Private Cloud (VPC) with CIDR block 10.16.0.0/16.
Subnets:
3 Public subnets for the Web tier in 3 different AZs.
3 Private subnets for the App tier in 3 different AZs.
3 Private subnets for the DB tier in 3 different AZs.
Internet Gateway (IGW): For internet access to public subnets.
NAT Gateway: To allow private subnets to access the internet securely.
Route Tables:
Public Route Table: Routes traffic from public subnets to the internet.
Private Route Table: Routes traffic from private subnets to the NAT Gateway.
Security Groups (Optional): Security groups can be configured to manage inbound and outbound traffic for EC2 instances, RDS databases, etc.
How to Deploy
1. Using the AWS Console
Go to the AWS Management Console.
Navigate to CloudFormation.
Select Create Stack.
Upload the YAML file from this repository.
Click Next and follow the instructions to deploy the stack.

aws cloudformation create-stack \
  --stack-name MultiTierArchitecture \
  --template-body file://path-to-your-cloudformation-file.yaml \
  --parameters ParameterKey=KeyName,ParameterValue=your-key-name \
  --capabilities CAPABILITY_NAMED_IAM
VPC (10.16.0.0/16)
│
├── Public Subnet A (10.16.48.0/20) - Web Tier
│   └── NAT Gateway, IGW
├── Private Subnet A (10.16.32.0/20) - App Tier
├── Private Subnet A (10.16.16.0/20) - DB Tier
│
├── Public Subnet B (10.16.112.0/20) - Web Tier
├── Private Subnet B (10.16.96.0/20) - App Tier
├── Private Subnet B (10.16.80.0/20) - DB Tier
│
└── Public Subnet C (10.16.176.0/20) - Web Tier
    ├── Private Subnet C (10.16.160.0/20) - App Tier
    └── Private Subnet C (10.16.144.0/20) - DB Tier
