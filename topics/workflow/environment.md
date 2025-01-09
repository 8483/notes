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
sudo timedatectl set-timezone CET;
```

# Tools

```bash
# curl, vim, tmux, git (Usually installed already)
sudo apt install curl vim tmux git -y;
```

# Node.js

**DO NOT install `nodejs` directly. Use `nvm`!**

> **MUST RESTART TERMINAL AFTER INSTALL FOR `nvm` TO WORK!**

```bash
# Download and install nvm
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash;
# MUST RESTART TERMINAL AFTER INSTALL FOR nvm TO WORK!!

# Install latest nodejs
nvm install node;

# Forward all the commands received as node to nodejs
sudo ln -sf "$(which node)" /usr/bin/node;
```

# Daemons

```bash
# Global nodemon
npm i -g nodemon;

# Global process manager
npm i -g pm2;

# Forward all the commands received as pm2 to whatever the fuck
sudo ln -sf "$(which pm2)" /usr/bin/pm2;
```

# Web Server - nginx

```bash
sudo apt install nginx -y;
```

# Database - MySQL

```bash
sudo apt install mysql-server;

sudo service mysql start
# sudo /etc/init.d/mysql start;
```

Add a password to the `root` user.

```bash
# Login
sudo mysql;

# Change root password
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';

# Reload the privileges after password change.
FLUSH PRIVILEGES;

# Login with password
sudo mysql -u root -p;
```

Change time zone.

```
sudo vim /etc/mysql/my.cnf
```

```
[mysqld]
default-time-zone = "+01:00"
```
