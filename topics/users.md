# Create

```bash
# Guided user creation
adduser <USERNAME>

# Add sudo access
usermod -aG sudo <USERNAME>

# Scripted user creation
useradd --uid <UID> --gid <GID> -m -s /usr/bin/zsh -d /var/www/<USERNAME> --password <PASSWORD> <USERNAME>
```

# Login

```bash
# Login as user
su - <USERNAME>

# Login as root
sudo -i

# Logout
CTRL + d # or exit
```

The root account is disabled by default in Ubuntu, so there is no root password, that's why `su` fails with an authentication error.

# Permissions

`ls -l` - Show file permissions.

`chown USER:GROUP FILENAME` - Change file owner.

`sudo usermod -a -G <group> <user>` - Add user to a group. Needs a logout.

`chmod` - Change file mode i.e. `rwx` permissions.

`r` - Read.  
`w` - Write.  
`x` - Execute.

| Permission | File Type | Owner | Group | Other |                             Meaning                             |
| :--------: | :-------: | :---: | :---: | :---: | :-------------------------------------------------------------: |
| -rwx------ |     -     |  rwx  |  ---  |  ---  |      File that only the owner can read, write and execute.      |
| drw-rw---- |     d     |  rw-  |  rw-  |  r--  | Directory that owner and group can read/write, others just read |

Owner permissions trump group ones.

`chmod +x FILE` - Add execute permission to owner, group and others.  
`chmod u+w FILE` - Add write permission to just the owner.  
`chmod o-r FILE` - Remove read permission to other users.

This can get very tedious for each permission (9 commands), so instead, we can set the permissions by using octal numbers.

First number is **owner**, second is **group**, and third is **others**.

`chmod 644 FILE` - Owner read/write, group read, others read.

| Dec | Permission           |
| :-: | -------------------- |
|  7  | Read, write, execute |
|  6  | Read, write          |
|  5  | Read, execute        |
|  4  | Read                 |
|  3  | Write, execute       |
|  2  | Write                |
|  1  | Execute              |
|  0  | No permissions       |

`7, 6, 5, 4, 0` - Most used.

A binary mask is used to determine the numbers. Add the column values for each 1 and the sum is the decimal number.

|   Type    | Read (4) | Write (2) | Execute (1) | Decimal |
| :-------: | :------: | :-------: | :---------: | :-----: |
| **User**  |    1     |     1     |      1      |    7    |
| **Group** |    1     |     0     |      1      |    5    |
| **Other** |    1     |     0     |      0      |    4    |

The number `754` would give the owner full permissions, the group read/execute, and just read for the others.
