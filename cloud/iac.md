
# Intro to Infrastructure as Code

## What problem needs solving?

- Setting up cloud infrastructure manually takes time.
- Repeating the same steps can cause human errors.
- Managing lots of servers manually is difficult.

## What have we automated?

- Installing software using Bash scripts.
- Deploying applications on EC2 instances.
- Setting up MongoDB databases.
- Starting applications automatically using User Data.

## Solving the problem

We can use Infrastructure as Code (IaC) to automate creating and managing cloud infrastructure instead of doing everything manually.

### What is IaC?

Infrastructure as Code (IaC) means using code to create and manage infrastructure such as:

- EC2 instances
- Networks
- Security groups
- S3 buckets

### Benefits of IaC?

- Saves time.
- Reduces human errors.
- Makes infrastructure easier to repeat.
- Makes changes easier to track.
- Allows infrastructure to be recreated quickly.

### When/where to use IaC

- When creating cloud infrastructure.
- When deploying applications.
- When managing multiple servers.
- When we need the same setup repeatedly.

### What are the tools available for IaC?

- Terraform
- AWS CloudFormation
- Pulumi

Terraform is a popular IaC tool that can work with AWS and other cloud providers.

### What is provisioning of infrastructure? Do CM tools do it?

Provisioning means creating the infrastructure needed to run an application.

For example:
- Creating an EC2 instance.
- Creating an S3 bucket.
- Setting up a network.

Tools like Terraform are mainly used for provisioning.

Configuration Management (CM) tools mainly configure existing servers, although some can also provision infrastructure.

### What is configuration management (CM)?

Configuration Management means setting up and maintaining servers after they have been created.

For example:
- Installing Nginx.
- Installing Node.js.
- Updating software.
- Managing configuration files.

Examples of CM tools:
- Ansible
- Puppet
- Chef

**Main difference:**

- IaC provisioning = Creates the infrastructure.
- Configuration Management = Configures and maintains the infrastructure.
