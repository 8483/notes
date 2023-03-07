# Files

**On a UNIX system, everything is a file. If something is not a file, it is a process.**

File extensions are meaningless. They are there for the user's sake.

Ubuntu is simply a collection (repository) of pre-compiled packages, running over a **kernel** (Big pile of software that knows how to make hardware do stuff).

```
Package = Plain text file > Compilation > Binary (.exe)
```

## File types

1. **Normal**. Images, text, config...
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

-   **boot** (bootloaders) - Everything the OS needs to boot.
-   **run** (runtime) - Runtime information stored in RAM i.e. not written to disk.
-   **sys** (system) - Interaction with the kernel. Similar to `run`, not written to disk.
-   **proc** (processes) - Pseudo files with information for system processes, created by the kernel. Every process has a directory named by the PID (process ID). Ex. `cat /proc/cpuinfo` gives CPU info.

## Devices

-   **dev** (devices) - Keyboard, mouse... - Other applications and drivers use this.
    -   **sda** - Hard disk (main). `sda1` would be a partition.
-   **media** (Device mounting - Automatic) - External hard drives, USBs.
-   **mnt** (Device mounting - Manual)
-   **cdrom** - Mounting point for CDs.

## System programs

-   **bin** (binaries) - Programs like `ls`, `cat`, `grep`...
-   **sbin** (system binaries) - Programs only admins use, single user mode.
-   **lib** (libraries) - Used by `bin` and `sbin`.

## Configuration

-   **etc** (etcetera) - System wide cofiguration files.
-   **var** (variable) - Files expected to grow in size, like logs, email databases, printer queues...

## User programs

-   **usr** - Applications installed and used by the user.
-   **opt** (optional) - Manually installed software from other vendors.
-   **tmp** (temp) - Temporary files ex. auto-saves.

## Users

-   **root** - Home folder for root users.
-   **home**. Home folders for each user, storing personal files.

## Other

-   **srv** (service) - Web server files accessible by external users.
-   **snap** - Self-contained Ubuntu apps.
