# Virtualbox

1. Create VM.
2. Mount and install Guest Additions.
3. **Reboot.**
4. Add clipboard settings.
5. Add host shared folder. It is under `/media/user/sf_folder_name`.
6. `sudo adduser username vboxsf` to access shared folder.
7. **Reboot.**

# Environment

**Sytem**

```bash
# Check timezone
timedatectl

# Change timezone
sudo timedatectl set-timezone EST # UTC, CET
```

**User**

```bash
# Add user
adduser <username>

# Switch to user
su - <username>
```

**Basic**

```bash
# Update
sudo apt update && sudo apt upgrade;

# curl, vim, tmux, git (Usually installed already)
sudo apt install curl vim tmux git -y;

# Download and install nvm
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash;

# Install latest nodejs
nvm install node

# Global nodemon
sudo npm i -g nodemon;

# MySQL (sudo mysql -u root)
sudo apt install mysql-server -y && mysql_secure_installation;
```

**Database**

```bash
# Change timezone
sudo vim /etc/mysql/my.cnf

# Change default-time-zone = "+01:00" to desired
```

**Web Server**

```bash
# nginx (web server)
sudo apt install nginx -y;

# Global process manager
sudo npm i -g pm2;
```

# Digital Ocean Update

To update Ubuntu, the droplet needs to be destroyed i.e. rebuilt with the new Linux distro.

After this, an email will be sent with the login credentials.
