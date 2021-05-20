Transfer files over SSH.

Only transfer the files that have changes or are missing. Much faster and secure than FTP.

# Options

```bash
-a   # Recursion and preserve everything.
-v   # Transfer log.
-h   # Display the output numbers in a human-readable format.
-e   # Specify remote shell.
-W   # Copy whole file, without checking for changes.

--progress   # Show the sync progress during transfer
```

# Exclude files/folders

```
rsync -av ./file1 ./folder1 --exclude 'folder1/node_modules/*' user@123.456.789.255:folder/
```

# Local > Remote

```bash
# Transfer everything from the current directory to a new folder in the remote home directory.
rsync -av . user@123.456.789.255:folder/

# Transfer folder1 and its content to remote home directory
rsync -av ./folder1 user@123.456.789.255:

# Transfer everything from folder1 without the folder itself to remote folder2 in remote home directory.
rsync -av ./folder1/ user@123.456.789.255:folder2/

# Transfer multiple files and folders.
rsync -av ./file1 ./file2 ./folder1 user@123.456.789.255:folder2/

# Another way
rsync -av -e 'ssh' ./folder1/ user@123.456.789.255:~/folder1/
```

# Remote > Local

Runs from local

```bash
rsync -av user@123.456.789.255:folder/ /path/to/local/storage
```

# Freezing

When the terminal freezes, fix it with this in a **new** terminal...

```bash
while sudo killall -CHLD ssh; do sleep 0.1; done;
```

# Virtual Machine

```bash
rsync -av -e "ssh -p PORT_NUMBER" <SOURCE> <DESTINATION>:<PATH>

rsync -av -e 'ssh -p 3022' . user@127.0.0.1:~/make-this_folder
```

# Backup

To copy multiple files, the command must be a string. This will copy the content from the locations to the single location the command is called from.

```bash
rsync -av -e 'ssh' 'root@123.456.789.255:/etc/nginx/nginx.conf /etc/letsencrypt/keys' .
```

This command will copy the `nginx.conf` file and the letsencrypt folder with the keys. The command must run as root for the keys.
