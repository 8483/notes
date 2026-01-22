# Windows Subsystem for Linux (WSL)

Allows using Linux directly in Windows, without double booting or virtual machines.

|                                | WSL 1                    | WSL 2                               |
| ------------------------------ | ------------------------ | ----------------------------------- |
| Architecture                   | Syscall translation      | Real Linux kernel in lightweight VM |
| Linux compatibility            | Partial                  | Near-complete                       |
| Kernel                         | No Linux kernel          | Full Linux kernel                   |
| Docker support                 | Unsupported              | Required / native                   |
| Filesystem (Linux files)       | NTFS                     | ext4 (virtual disk)                 |
| Linux I/O performance          | Slower                   | Much faster                         |
| Windows file access (`/mnt/c`) | Faster                   | Slower than WSL 1                   |
| Networking                     | Shared host IP           | Virtualized network, separate IP    |
| System services                | Limited                  | Fully supported                     |
| cgroups / namespaces           | Not supported            | Fully supported                     |
| Resource usage                 | Lower overhead           | Slightly higher, dynamic            |
| Use case                       | Legacy / Windows-centric | Modern Linux development            |
| Recommendation                 | Avoid                    | Default choice                      |

# Commands

Ran in windows powershell, **NOT** the linux terminal.

```bash
# The actual version of each installed distro
wsl -l -v

# The default version for newly created distros
wsl --version

# Forces the chosen distro to run under WSL 2. Converts it from WSL 1 if applicable.
wsl --set-version Ubuntu-24.04 2

# Sets WSL 2 as the default for newly created distros only. Does not modify existing ones.
wsl --set-default-version 2

# Updates the WSL kernel and components used by WSL 2. No effect on WSL 1 distros.
wsl --update

# Stops all running WSL distros and the WSL virtual machine. Clears state. Required after version or kernel changes.
wsl --shutdown
```

# Install

via PowerShell

```text
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
wsl --set-default-version 2
wsl --install -d Ubuntu-24.04

wsl -l -v

wsl --set-version Ubuntu-24.04 2
```

via Linux terminal

```bash
lsb_release -a
```

# Files

```bash
# Filesystem
C:\Users\User\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu22.04LTS_79rhkp1fndgsc\LocalState\rootfs

# Terminal .exe
C:\Users\User\AppData\Local\Microsoft\WindowsApps\CanonicalGroupLimited.Ubuntu22.04LTS_79rhkp1fndgsc

# My Computer (C:/D:)
cd /mnt/
```
