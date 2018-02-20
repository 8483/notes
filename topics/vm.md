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

# vim and curl
sudo apt install vim curl;

# Node
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash - && sudo apt install nodejs -y;

# MySQL
sudo apt install mysql-server -y && mysql_secure_installation;

# Atom
sudo add-apt-repository ppa:webupd8team/atom && sudo apt update && sudo apt install atom -y;
```
