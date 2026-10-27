# Deploying Tic Tac Toe With MongoDB on AWS EC2

## 1. Create the MongoDB Instance

1. Launch an **Ubuntu 22.04 EC2 instance**.
2. Allow ports **22 (SSH)** and **27017 (MongoDB)**.
3. Connect using Git Bash:

```bash
ssh -i "your-key.pem" ubuntu@DB-PUBLIC-IP
```

## 2. Install MongoDB

Update Ubuntu:

```bash
sudo apt update -y
sudo apt upgrade -y
```

Add the MongoDB repository:

```bash
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | sudo gpg --dearmor -o /usr/share/keyrings/mongodb-server-7.0.gpg
```

```bash
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

Install MongoDB:

```bash
sudo apt update -y

sudo DEBIAN_FRONTEND=noninteractive apt-get install -y \
mongodb-org=7.0.22 \
mongodb-org-database=7.0.22 \
mongodb-org-server=7.0.22 \
mongodb-mongosh \
mongodb-org-shell=7.0.22 \
mongodb-org-mongos=7.0.22 \
mongodb-org-tools=7.0.22 \
mongodb-org-database-tools-extra=7.0.22
```

## 3. Configure MongoDB

Open the configuration file:

```bash
sudo nano /etc/mongod.conf
```

Change `bindIp` to:

```yaml
bindIp: 0.0.0.0
```

Start MongoDB:

```bash
sudo systemctl start mongod
sudo systemctl enable mongod
sudo systemctl status mongod
```

## 4. Create the App Instance

1. Launch a second EC2 instance for Tic Tac Toe.
2. Allow ports **22, 80 and 3000**.
3. Connect using Git Bash:

```bash
ssh -i "your-key.pem" ubuntu@APP-PUBLIC-IP
```

## 5. Connect the App to MongoDB

Navigate to the application folder containing `index.js`.

Set the MongoDB connection:

```bash
export MONGODB_URI=mongodb://DB-PUBLIC-IP:27017/tictactoe
```

Verify:

```bash
printenv MONGODB_URI
```

Restart the application:

```bash
pm2 kill
npm install
pm2 start index.js --name tic
pm2 status
```

## 6. Test the Application

Open the app instance's public IP in a browser:

```text
http://APP-PUBLIC-IP
```