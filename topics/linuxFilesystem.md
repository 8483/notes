# Files

**On a UNIX system, everything is a file. If something is not a file, it is a process.**

File extensions are meaningless. They are there for the user's sake.

Ubuntu is simply a collection (repository) of pre-compiled packages, running over a **kernel** (Big pile of software that knows how to make hardware do stuff).

```
Package = Plain text file > Compilation > Binary (.exe)
```

## File types

1. **File**. Images, text, config...
2. **Directory**. File pointing to files.
3. **Link**. Shortcut/Redirect.
4. **Pipe**. Use a process output as input for another.
5. **Character Device**. Input/Output files Ex. Teminal.
6. **Block**. Used for block devices. Ex. Hard Disk.
7. **Socket**. Used for Interprocess Communication.
    - **Unix Socket**. Local machine only (Superfast). Ex. Nginx communicates with a PHP interpreter.
    - **TCP Socket**. Exposed to network (Slower). Ex. Nginx communicates with a website visitor.

## Interprocess Communication (IPC)

Special files like **sockets** that allow processes to communicate with each other without dangerously sharing memory.

**Sockets** are files where processes can write stuff, and other processes can listen in real time.

# Filesystem

## OS

```bash
/boot      # Bootloaders - Everything the OS needs to boot.
/run       # Runtime - Information stored in RAM i.e. not written to disk.
/sys       # System - Kernel interaction. Similar to /run.
/proc      # Processes - Pseudo files with information for system processes, created by the kernel. Every process has a directory named by the PID (process ID). Ex. cat /proc/cpuinfo gives CPU info.
```

## Devices

```bash
/dev           # Devices ex. mouse - Other programs and drivers use this.
    /dev/sda   # Hard disk (main). `sda1` would be a partition.
/media         # Device mounting (Automatic) - External hard drives, USBs.
/mnt           # Device mounting (Manual)
/cdrom         # Mounting point for CDs.
```

## Programs

```bash
# System

/bin        # Binaries - Programs like ls, cat, grep...
/sbin       # System binaries - Admin only programs.
/lib        # Libraries - Used by bin and sbin.

# User

/usr        # Applications installed and used by the user.
/opt        # Optional - Manually installed software from other vendors.
/tmp        # Temp - Temporary files ex. auto-saves.
```

## Configuration

```bash
/etc        # Etcetera - System wide cofiguration files.
/var        # Variable - Files expected to grow in size ex. logs, databases.
```

## Users

```bash
/root        # - Home folder for root users.
/home        #. Home folders for each user, storing personal files.
```

## Other

```bash
/srv        # Service - Web server files accessible by external users.
/snap       # Self-contained Ubuntu apps.
```
