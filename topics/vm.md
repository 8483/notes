# Virtualbox

1. Create VM.
2. Mount and install Guest Additions.
3. **Reboot.**
4. Add clipboard settings.
5. Add host shared folder. It is under `/media/user/sf_folder_name`.
6. `sudo adduser username vboxsf` to access shared folder.
7. **Reboot.**

# Environment

User
```bash
# Add user
adduser <username>

# Add sudo access
usermod -aG sudo <username>

# Switch to user
su - <username>
```

Basic

```bash
# Update
sudo apt update && sudo apt upgrade;

# curl, vim, tmux, git (Usually installed already)
sudo apt install curl vim tmux git -y;

# Node
sudo apt install nodejs npm -y;

# Global nodemon
sudo npm i -g nodemon;

# MySQL (sudo mysql -u root)
sudo apt install mysql-server -y && mysql_secure_installation;
```

Web Server

```bash
# nginx (web server)
sudo apt install nginx -y;

# Global forever
sudo npm i -g forever;
```

# Digital Ocean Update

To update Ubuntu, the droplet needs to be destroyed i.e. rebuilt with the new Linux distro.

After this, an email will be sent with the login credentials.