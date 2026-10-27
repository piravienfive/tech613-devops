# Deploying Tic Tac Toe Using an AWS AMI

## 1. Create the AMI

1. Go to **AWS EC2 > Instances**.
2. Select the working EC2 instance.
3. Click **Actions > Image and templates > Create image**.
4. Name it `sparta-tictactoe-ami`.
5. Click **Create image**.
6. Go to **EC2 > AMIs** and wait until the status is **Available**.

## 2. Launch a New Instance From the AMI

1. Go to **EC2 > AMIs**.
2. Select the AMI.
3. Click **Launch instance from AMI**.
4. Choose an instance type and key pair.
5. Enable a public IPv4 address.
6. Configure the security group to allow:
   - **SSH (22)** 
   - **HTTP (80)** 
   - **Custom TCP (3000)** 
7. Launch the instance.

## 3. Connect Using SSH

1. Select the new EC2 instance.
2. Click **Connect**.
3. Copy the SSH connection command.
4. Change `root` to `ubuntu` in the command.

For example:

```bash
ssh -i "your-key.pem" ubuntu@YOUR-PUBLIC-IP
```

5. Run the command in the terminal to connect to the instance.

## 4. Start the Tic Tac Toe Application

Navigate to the folder containing `index.js`:

```bash
cd ~/repo/app
```

Start the application using PM2:

```bash
pm2 start index.js --name tic
```

Check that the application is running:

```bash
pm2 status
```

The application should show **online**.

## 5. Check Nginx

Check that Nginx is running:

```bash
sudo systemctl status nginx
```

Nginx forwards traffic from port 80 to the Node.js application on port 3000.

## 6. Test the Website

1. Copy the new EC2 instance's **Public IPv4 address**.
2. Open a web browser.
3. Enter:

```text
http://YOUR-PUBLIC-IP
```

The Tic Tac Toe game should appear.

You can also test the application directly on port 3000:

```text
http://YOUR-PUBLIC-IP:3000
```

## Conclusion

I created an AMI from a working EC2 instance and launched a new instance using that image.

I configured ports 22, 80 and 3000, connected using SSH as the `ubuntu` user, and started the Tic Tac Toe application using PM2.

This helped me understand how AMIs, EC2, SSH, PM2 and Nginx work together to deploy an application.