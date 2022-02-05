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

This is used to log in without writing the password each time. Also, it is much more secure.

Generate private/public keys in `/home/user/.ssh`

```
ssh-keygen
```

-   Private key `id_rsa` stays in the host machine in `~/.ssh`.
-   Public key `id_rsa.pub` goes on the server in a `~/.ssh/authorized_keys` file.

Simple as that, the next ssh is password-less.

To transfer a public key, use this:

```bash
ssh-copy-id user@255.255.255.255
```

This automates the manual creation of an `~/.ssh/authorized_keys` file and putting the `id_rsa.pub` key in.
