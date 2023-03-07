# Login

```bash
# Login as user
su - <USERNAME>

# Login as root
sudo -i

# Logout
CTRL + d # or exit

# Change password
sudo passwd <USERNAME>
```

# Add

```bash
# Guided user creation
adduser <USERNAME>

# Add sudo access
usermod -aG sudo <USERNAME>

# remove sudo access
sudo deluser <USERNAME> sudo

# Scripted user creation
useradd --uid <UID> --gid <GID> -m -s /usr/bin/zsh -d /var/www/<USERNAME> --password <PASSWORD> <USERNAME>
```

# Remove

```bash
# Remove user
sudo deluser --remove-home <USERNAME>

# Check if user exists
id <USERNAME>

# Kill all processes by user
pkill -u <USERNAME>
```

The root account is disabled by default in Ubuntu, so there is no root password, that's why `su` fails with an authentication error.

# Permissions

```bash
ls -l

#    permission   links   user    group    size     changed        file
#    -rwxrwxrwx     1     foo      bar      45    Nov 13 22:51   script.js
```

| Permission |     Type      | Owner | Group | Other |                      Meaning                      |
| :--------: | :-----------: | :---: | :---: | :---: | :-----------------------------------------------: |
| -rwx------ |   - (file)    |  rwx  |  ---  |  ---  | File only the owner can read, write and execute.  |
| drw-rw---- | d (directory) |  rw-  |  rw-  |  r--  | Owner and group can read/write, others just read. |

```bash
# Change file owner
chown user:group file_name

# Add user to a group. Needs a logout
sudo usermod -a -G group user
```

# chmod

```bash
# Change file mode i.e. `rwx` permissions.
chmod

# Add EXECUTE permission to owner, group and others.
chmod +x FILE

# Add WRITE permission to just the owner.
chmod u+w FILE

# Remove read permission to other users.
chmod o-r FILE
```

Adding permission one by one can get very tedious (9 commands), so instead, we can set the permissions by using octal numbers.

```bash
chmod 754 FILE
```

|                |        Owner         |     Group     | Others |
| :------------: | :------------------: | :-----------: | :----: |
|   **Octal**    |          7           |       5       |   4    |
|   **Chars**    |         rwx          |      r-x      |  r--   |
| **Permission** | Read, write, execute | Read, execute |  Read  |

A binary mask is used to determine the numbers. Add the column values for each 1 and the sum is the decimal number.

| Dec | Permission           | RWX | Read (4) | Write (2) | Execute (1) | Decimal |
| :-: | -------------------- | :-: | :------: | :-------: | :---------: | :-----: |
|  7  | Read, write, execute | rwx |    1     |     1     |      1      |    7    |
|  6  | Read, write          | rw- |    1     |     1     |      0      |    6    |
|  5  | Read, execute        | r-x |    1     |     0     |      1      |    5    |
|  4  | Read                 | r-- |    1     |     0     |      0      |    4    |
|  3  | Write, execute       | -wx |    0     |     1     |      1      |    3    |
|  2  | Write                | -w- |    0     |     1     |      0      |    2    |
|  1  | Execute              | --x |    0     |     0     |      1      |    1    |
|  0  | No permissions       | --- |    0     |     0     |      0      |    0    |
