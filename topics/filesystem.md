# Filesystem

#### Directories

**root** has the following directories:  
- **etc**. Configuration files.  
- **proc**. System information.  
- **home**. All the users.
- **boot**. Operating system lives here.  
- **dev**. Where devices are mounted.  `sda` is main disk, `sdaX` are partitions (Mounted as block files).  

`tree DIRECTORY` - Show directory tree. `tree .` for current. Needs to be donwloaded with `sudo apt-get install tree`.  

#### Files

Plain text file > Compilation > Binary (.exe) i.e. package. Ubuntu is simply a collection (repository) of pre-compiled packages, running over a kernel (Big pile of software that knows how to make the hardware do stuff).  

**"On a UNIX system, everything is a file; if something is not a file, it is a process."** (directory is a file containing names of other files)  

File extensions are meaningless. They are there for the user's sake.

There are 7 types of files, which can be recognized by using `ls -l` and looking at the first bit:  
1. **Normal**. Images, text, config...
2. **Directory**. File pointing to files.
3. **Link**. Shortcut/Redirect.
4. **Pipe**. Use a process output as input for another.
5. **Character Device**. Input/Output files Ex. Teminal.
6. **Block**. Used for block devices. Ex. Hard Disk.
7. **Socket**. Used for Interprocess Communication.
    - **Unix Socket**. Local machine only (Superfast). Ex. Nginx communicates with a PHP interpreter.  
    - **TCP Socket**. Exposed to network (Slower). Ex. Nginx communicates with a website visitor.

#### Interprocess Communication (IPC)

In Linux, **everything is a file**. There are special files like **sockets** that allow processes to communicate with each other without dangerously sharing memory.  

**Sockets** are files where processes can write stuff, and other processes can listen in real time.  
