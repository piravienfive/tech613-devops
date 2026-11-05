# AWS VPC Deployment

## 1. Aim

Create a custom VPC for our application and MongoDB database.

We will create:
- 1 VPC
- 1 public subnet
- 1 private subnet
- 1 Internet Gateway
- 2 route tables
- 2 security groups

## 2. Create Your VPC

1. Open AWS Console.
2. Search for VPC.
3. Click Your VPCs.
4. Click Create VPC.
5. Select VPC only.
6. Enter your VPC name.
7. IPv4 CIDR: `10.0.0.0/16`.
8. Select No IPv6 CIDR block.
9. Tenancy: Default.
10. Click Create VPC.

## 3. Create Your Public and Private Subnet

1. Go to VPC > Subnets.
2. Click Create subnet.
3. Select your VPC.
4. Enter your public subnet name.
5. Select an Availability Zone.
6. IPv4 CIDR: `10.0.2.0/24`.
7. Click add subnet
8.  Enter your private subnet name.
9.  Select an Availability Zone.
10. IPv4 CIDR: `10.0.3.0/24`.
11. Click Create subnet.


## 4. Create Your Public Route Table

1. Go to VPC > Route Tables.
2. Click Create route table.
3. Enter your public route table name.
4. Select your VPC.
5. Click Create route table.

### Associate Your Public Subnet

1. Open the Subnet associations tab.
2. Click Edit subnet associations.
3. Select your public subnet.
4. Click Save associations.



## 5. Create Your Internet Gateway

1. Go to VPC > Internet Gateways.
2. Click Create internet gateway.
3. Enter your Internet Gateway name.
4. Click Create internet gateway.
5. Select your Internet Gateway.
6. Click Actions > Attach to VPC.
7. Select your VPC.
8. Click Attach internet gateway.

### Add Internet Route

1. Select your public route table.
2. Open the Routes tab.
3. Click Edit routes.
4. Click Add route.
5. Destination: `0.0.0.0/0`.
6. Target: Internet Gateway.
7. Select your Internet Gateway.
8. Click Save changes.


## 6. Create Your App Security Group

1. Go to EC2 > Security Groups.
2. Click Create security group.
3. Enter your app security group name.
4. Enter a description.
5. Select your VPC.

### Inbound Rules

**Rule 1 - SSH**
- Type: SSH
- Port: 22
- Source: My IP

**Rule 2 - HTTP**
- Type: HTTP
- Port: 80
- Source: Anywhere IPv4

6. Leave the default outbound rule.
7. Click Create security group.

## 7. Create Your Database Security Group

1. Go to EC2 > Security Groups.
2. Click Create security group.
3. Enter your database security group name.
4. Enter a description.
5. Select your VPC.

### Inbound Rules

**Rule 1 - MongoDB**
- Type: Custom TCP
- Port: 27017
- Source: Custom
- Select your app security group.

6. Click Create security group.

Only your app security group should be allowed to access MongoDB.