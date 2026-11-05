
# Intro to Terraform

## What is Terraform?

- Terraform is an Infrastructure as Code (IaC) tool.
- It creates and manages cloud infrastructure automatically.
- It can create EC2 instances, S3 buckets, networks and security groups.
- It uses a language called HCL (HashiCorp Configuration Language).
- Terraform is an orchestration and infrastructure provisioning tool.

## Why use Terraform?

- Saves time by automating infrastructure.
- Reduces human errors.
- Makes infrastructure easy to repeat.
- Works with different cloud providers like AWS, Azure and GCP.
- Uses declarative code, meaning we describe what we want rather than every step to create it.
- Makes it easier to create and destroy resources.

## Is Terraform open source?

- Terraform originally used an open-source licence.
- In 2023, HashiCorp changed to the Business Source License (BSL).
- This places restrictions on some commercial uses.
- OpenTofu is an open-source alternative to Terraform.

## Alternatives to Terraform

- AWS CloudFormation
- Pulumi
- Azure Bicep
- OpenTofu

## Who uses Terraform?

Terraform is used across many industries, including:

- Technology companies
- Banks and financial institutions
- Healthcare
- Retail
- Government
- Gaming

## What is orchestration in IaC?

Orchestration means automatically managing multiple infrastructure resources together.

For example:
- Creating an EC2 instance.
- Creating a security group.
- Connecting the EC2 instance to the security group.

## How does Terraform act as an orchestrator?

- Creates resources in the correct order.
- Understands dependencies between resources.
- Connects resources together.
- Can update or destroy infrastructure.

## How does Terraform access AWS?

Terraform needs AWS credentials to create resources.

### 1. Environment Variables

We can set:

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`

Terraform can automatically use these variables.

### 2. AWS CLI

Run:

```bash
aws configure
```

This saves AWS credentials locally.

Terraform can use these saved credentials.

### 3. IAM Roles

- Used when Terraform runs on an EC2 instance.
- Provides permissions without storing access keys.
- Generally the safest option for EC2.

### Important

Never hard-code AWS access keys directly into Terraform files.

## How does Terraform access Azure?

1. Install Azure CLI.
2. Log in using:

```bash
az login
```

Terraform can use the Azure login credentials.

## How does Terraform access GCP?

1. Install Google Cloud CLI.
2. Run:

```bash
gcloud init
```

3. Set up Application Default Credentials:

```bash
gcloud auth application-default login
```

Terraform can use these credentials.

## Why use Terraform for different environments?

### Production

- Create infrastructure for real users.
- Easily scale infrastructure when needed.
- Keep infrastructure consistent.

### Development and Testing

- Quickly create environments for testing.
- Use similar infrastructure to production.
- Destroy resources when no longer needed to save money.

## Terraform vs Ansible

- Terraform creates and manages infrastructure.
- Ansible configures servers and installs software.

Example:

- Terraform creates an EC2 instance.
- Ansible installs Nginx and Node.js on that instance.

## What I Learned

- Terraform is an IaC tool.
- It automates infrastructure provisioning.
- It uses HCL to describe infrastructure.
- It works with multiple cloud providers.
- It manages dependencies between resources.
- It makes infrastructure easier to recreate and manage.
