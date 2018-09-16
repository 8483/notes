# Virtualbox

1. Create VM.
2. Mount and install Guest Additions.
3. **Reboot.**
4. Add clipboard settings.
5. Add host shared folder. It is under `/media/user/sf_folder_name`.
6. `sudo adduser username vboxsf` to access shared folder.
7. **Reboot.**

# Packages

### Update

```bash
# Update
sudo apt update;

# curl, vim, tmux.
sudo apt install curl vim tmux;

# Node
sudo apt install nodejs git -y;

# MySQL
sudo apt install mysql-server -y && mysql_secure_installation;
```
