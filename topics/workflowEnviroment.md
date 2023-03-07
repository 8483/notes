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

# User

```bash
# Add user
adduser <username>

# Switch to user
su - <username>
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
sudo npm i -g nodemon;

# Global process manager
sudo npm i -g pm2;
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
