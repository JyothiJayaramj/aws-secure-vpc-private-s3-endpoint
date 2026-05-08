# aws-secure-vpc-private-s3-endpoint
A secure, production-ready AWS VPC architecture with public and private subnets, demonstrating private access to S3 using VPC Gateway Endpoint. No internet exposure for private resources.  Built as part of AWS Solutions Architect learning journey 

## Project Overview: AWS VPC with Private S3 Access using VPC Endpoint

Key Services: VPC, Subnets, Internet Gateway, Route Tables, EC2, Security Groups, S3, VPC Endpoint (Gateway type for S3)

Main Goal:
The private EC2 instance (no public IP, no internet access) should access the S3 bucket without going over the public internet. This is achieved using a VPC Endpoint (Gateway type for S3).

<img width="509" height="316" alt="image" src="https://github.com/user-attachments/assets/8d48470e-52e5-452a-87f2-dc0234e5a0f1" />


## **Step 1: Create VPC**

1. Go to **VPC Console → Create VPC and More**

- Give VPC and keep everything default if your not specific about it and click on Create VPC
- keep NAT and VPC endpoints as None for now

<img width="1313" height="544" alt="image" src="https://github.com/user-attachments/assets/21d9b219-6d1b-48f1-a73d-fdf36aeff3f2" />


<img width="1304" height="560" alt="image" src="https://github.com/user-attachments/assets/c972e119-17d9-4b9d-aeb6-22761e0a6210" />


<aside>
💡

This will Create VPC,2 public and 2 private Subnets ,IGW and also  attach route tables and IGW to the subnet, VPC

</aside>

## Step 2: Launch EC2 Instances

**Public EC2** (in Public Subnet):

- Assign **Auto-assign Public IP**: Enable
- Security Group: Allow SSH (port 22) from your IP

**Private EC2** (in Private Subnet):

- **No public IP**
- Same key pair
- Security Group: Allow SSH from Public EC2’s Security Group (or your IP for testing)

**Tip**: Use same key pair for both instances.

<img width="1304" height="560" alt="image" src="https://github.com/user-attachments/assets/c86938b4-15e0-4b69-8a69-4e4dd4c53d48" />


<img width="1304" height="560" alt="image" src="https://github.com/user-attachments/assets/90437d64-85a2-4af3-bb10-0c8ba5f886c0" />


## Step 3 : SSH into the ec2 instance

- SSH into the EC2 instance created in the public subnet

<img width="715" height="393" alt="image" src="https://github.com/user-attachments/assets/a2cf1f79-5ce5-4fc7-8e42-096a379ece99" />


- create and copy the key pair file in the public ec2 intance

<img width="715" height="393" alt="image" src="https://github.com/user-attachments/assets/8f8cebc0-8619-46c7-bb26-43610afaa444" />


- Login into the private ec2 instance inside public ec2 instance using key pair you created

<img width="715" height="393" alt="image" src="https://github.com/user-attachments/assets/0f72615f-59af-4d8d-a8e3-2e779402309d" />


- **Perform the AWS configure in private ec2 instance.** `aws configure` is used to connect your AWS CLI to your AWS account by saving your access keys and default region.

<img width="644" height="214" alt="image" src="https://github.com/user-attachments/assets/dcebf2c6-7a37-4d3e-95a4-b6402406b4fb" />


## **Step 4: Create S3 Bucket**

1. Create an S3 bucket (any name, same region as VPC).
2. Upload a sample file for testing. ****

<img width="1305" height="512" alt="image" src="https://github.com/user-attachments/assets/9fef4eec-b923-406e-89cf-b80dda694a4a" />


## Step 5: Create VPC Endpoint for S3

- Give endpoint name and select the Type of VPC end point as AWS service as we want work AWS S3 service

<img width="1325" height="570" alt="image" src="https://github.com/user-attachments/assets/e7e1d699-ce32-4c1e-a2f1-e3a889a19c60" />


- Select the more specific service you want to configure with VPC end-point

<img width="1328" height="458" alt="image" src="https://github.com/user-attachments/assets/14b309e2-b07d-4c3a-8452-cfd4957ef1aa" />


- the **Route Table** determines which subnet traffic should be redirected to the endpoint instead of going through the internet/NAT gateway.
- Route tables: Select only Private Route Table only as we want our private ec2 instance to communicate with s3 without public internet

<img width="1337" height="524" alt="image" src="https://github.com/user-attachments/assets/e6c9020c-9223-4982-a5ac-86811faba79f" />


- Gateway Endpoint (S3 & DynamoDB only) adds a route in the selected route tables pointing to the endpoint

## Step 7: Testing

Expected: Private EC2 can access S3 but cannot access public internet.

<img width="460" height="88" alt="image" src="https://github.com/user-attachments/assets/966b17be-fe3d-4412-8ad7-0226d4806a91" />


<img width="538" height="111" alt="image" src="https://github.com/user-attachments/assets/a6e89809-2a1d-4690-8f1a-76138c3c415d" />

