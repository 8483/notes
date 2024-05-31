# Install

```bash
sudo apt-get install openssh-server
```

# Connect

```bash
ssh -p PORT USER@SERVER_IP

# Execute one command and exit.
ssh -p PORT USER@SERVER_IP ls
```

SSH in Virtual Machine needs a port forwarding rule in network settings for the VM. Name `ssh`, host port `3022`, guest port `22`.

```bash
# SSH to local VM via port forwarding.
ssh -p 3022 user@127.0.0.1
```

# sudo command

```bash
# It will ask for the root password in the remote machine
ssh -t user@255.255.255.255 "sudo command"
```

# Alias

```bash
/etc/ssh/ssh_config   # system-wide
~/.ssh/config         # per user (better)

# Add this in the config.
Host server_name
  Port 22
  User user
  HostName 123.456.789.255

# Connect with this now.
ssh server_name
```

# SSH with RSA key

Access servers with a **public/private** key instead of a password. This is a **much** more secure method.

Transfer the **public** key `id_rsa.pub` to the remote server, in the `~/.ssh/authorized_keys` file, with this command:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub user@123.456.789.255
```

**If the keys don't exist** on local machine, generate them:

```bash
ssh-keygen

# keys are stored in /home/user/.ssh
ls ~/.ssh
```

The **private** key `id_rsa` stays in the local machine.
