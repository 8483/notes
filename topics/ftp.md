# File Transfer Protocol (FTP)

Used to transfer computer files between a remote and a local machine.

> `FTP` is insecure. Use `SFTP` instead.

`SFTP` is part of the SSH (Secure Shell) service, which is usually already installed and running on Linux.

As long as the server has `sshd` running and the user has `SSH` access, you can connect via `SFTP`.

### **Server**

1. Create an FTP user (just a normal Linux user).

```bash
sudo adduser john
```

You can check the existing users with `less /etc/passwd`.

2. Add password.

```bash
sudo passwd john
```

3. Limit user access to a specific directory.

```bash
sudo usermod -d /home/user/folder john
```

4. Give read/write permissions (Might not be needed).

```bash
sudo chown john:john /home/user/folder
sudo chmod -R 755 /home/user/folder
```

### **Client**

You can now access the directory remotely with an `FTP` client. A popular one is Filezilla.

Just add you server's IP address in the `host` filed, along with the username and password created above.

Make sure you are using `SFTP` and port `22` (default SSH port) for the connection. It should use it automatically.
