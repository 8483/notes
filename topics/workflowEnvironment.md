# User

```bash
# Login as root with password
ssh root@123.456.789.255

# Add user
adduser <user>

# Add sudo access
usermod -aG sudo <user>

# Switch to user
su - <user>
```

# SSH

```bash
# Transfer the public key id_rsa.pub to the remote server
ssh-copy-id -i user@123.456.789.255

# If they private/public keys don't exist
ssh-keygen

# Connect to server
ssh user@123.456.789.255
```

# System

```bash
# Update OS
sudo apt update && sudo apt upgrade;

# Check version
lsb_release -a

# Check timezone
timedatectl

# Change timezone
sudo timedatectl set-timezone EST # UTC, CET
```

# Tools

```bash
# curl, vim, tmux, git (Usually installed already)
sudo apt install curl vim tmux git -y;
```

# Node.js

**DO NOT install `nodejs` directly. Use `nvm`!**

```bash
# Download and install nvm
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash;

# Install latest nodejs
nvm install node

# Global nodemon
npm i -g nodemon;

# Global process manager
npm i -g pm2;
```

# Web Server - nginx

```bash
sudo apt install nginx -y;
```

# Database - MySQL

```bash
# MySQL (sudo mysql -u root)
sudo apt install mysql-server -y && mysql_secure_installation;

# Change timezone
sudo vim /etc/mysql/my.cnf

# Change default-time-zone = "+01:00" to desired
```
