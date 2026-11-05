# AWS Auto Scaling Group (ASG)

## 1. What is Auto Scaling?

Auto Scaling automatically manages the number of EC2 instances running our application.

It helps us:
- Handle more traffic.
- Replace unhealthy instances.
- Keep the application available.

## 2. Two Types of Scaling

### Vertical Scaling

Making one server more powerful.

Example: Changing from `t3.micro` to `t3.large`.

Used when we need more CPU or RAM.

### Horizontal Scaling

Adding more servers.

Example: Increasing from 2 EC2 instances to 3.

Used to handle more traffic and improve availability.

## 3. Auto Scaling Diagram

```text
                  Users
                    |
                    v
           Application Load Balancer
                    |
                    v
               Target Group
                    |
             ---------------
             |             |
             v             v
          EC2 1          EC2 2
             |             |
             ---------------
                    |
            Auto Scaling Group
                    |
             High CPU Usage
                    |
                    v
                  EC2 3
         (if maximum allows)
```

## 4. Create a Launch Template

1. Go to AWS > EC2.
2. Select Launch Templates.
3. Click Create launch template.
4. Name it `tic-launch-template`.
5. Select the AMI containing the working Tic Tac Toe app.
6. Select the instance type.
7. Select the key pair.
8. Select the app security group.
9. Create the launch template.

Make sure the application starts automatically when EC2 boots.


## 5. Create the Auto Scaling Group

1. Go to EC2 > Auto Scaling Groups.
2. Click Create Auto Scaling group.
3. Name it `tic-asg`.
4. Select `tic-launch-template`.
5. Click Next.
6. Select all 3 subnets in different Availability Zones.
7. Click Next.


### Create the Load Balancer Inside the ASG Setup

1. In the Load balancing section, choose Attach to a new load balancer.
2. Select Application Load Balancer.
3. Choose Internet-facing.
4. Select an HTTP listener on port 80.
5. Select the option to create a new target group.
6. Use HTTP port 80 for the target group.
7. Continue to the next step.

AWS will create the Load Balancer and Target Group as part of the ASG setup.

Make sure the Load Balancer security group allows HTTP port 80.

### Enable Health Checks

1. Enable Elastic Load Balancing health checks.
2. Continue to the next step.


### Set Group Size

- Desired capacity: **2**
- Minimum capacity: **2**
- Maximum capacity: **3**


### Configure Scaling Policy

1. Select Target tracking scaling policy.
2. Choose Average CPU utilization.
3. Set the target value to **50%**.
4. Select 'No policy' for Instance maintenance policy.
5. Select 'Enable group metrics collection within CloudWatch' in additional settings.
6. Review the settings.
7. Click Create Auto Scaling group.


## 6. Verify the ASG

1. Go to EC2 > Auto Scaling Groups.
2. Select `tic-asg`.
3. Open Instance management.
4. Check that 2 instances are running.
5. Check that the instances become healthy.


### Test the Load Balancer DNS

1. Open the ASG Details tab.
2. Find the Load balancing section.
3. Open the linked Load Balancer.
4. Copy its DNS name.
5. Paste the DNS into your browser using HTTP.
6. Check that Tic Tac Toe loads.

Example:

```text
http://your-load-balancer-dns-name
```


## 7. Make the Third Instance Appear

This is an optional test. Only change the maximum capacity if Ramon allows it.

### Generate CPU Load

SSH into both app EC2 instances.

Install the stress tool:

```bash
sudo apt-get update
sudo apt-get install -y stress
```

Run this on both instances:

```bash
stress --cpu 2 --timeout 300
```

This generates CPU load for 5 minutes.

The ASG uses average CPU usage across its instances, so testing both helps trigger scaling.


### Check the Third Instance

1. Go to EC2 > Auto Scaling Groups.
2. Select `tic-asg`.
3. Open the Activity tab.
4. Look for a new instance launch.
5. Open Instance management.
6. Check whether the third instance appears.

The scaling process may take several minutes.


Before 18:00:

1. Go to EC2 > Auto Scaling Groups.
2. Select `tic-asg`.
3. Delete the ASG.
4. Check that its EC2 instances terminate.
5. Delete the unused Load Balancer then the Target Group to avoid extra charges.
