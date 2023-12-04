# Windows Subsystem for Linux

## Table of Contents
- [Windows Subsystem for Linux](#windows-subsystem-for-linux)
  - [Table of Contents](#table-of-contents)
  - [Installing Ubuntu \& Ubuntu Desktop](#installing-ubuntu--ubuntu-desktop)
  - [Basic commands for navigating and interacting with WSL](#basic-commands-for-navigating-and-interacting-with-wsl)
    - [1. Listing Distributions](#1-listing-distributions)
    - [2. Launching a Distribution](#2-launching-a-distribution)
    - [3. Navigating the File System](#3-navigating-the-file-system)
    - [4. Windows File System Integration](#4-windows-file-system-integration)
    - [5. Installing Packages](#5-installing-packages)
    - [6. Running Command-Line Tools](#6-running-command-line-tools)
    - [7. Exiting WSL](#7-exiting-wsl)
  - [Installing Linux apps that supports GUI](#installing-linux-apps-that-supports-gui)
  - [File System Integration](#file-system-integration)
    - [1. Accessing Windows Files from WSL](#1-accessing-windows-files-from-wsl)
    - [2. File Permissions and Ownership in WSL](#2-file-permissions-and-ownership-in-wsl)

## Installing Ubuntu & Ubuntu Desktop
- Update WSL to the latest
``` powershell
wsl --update
wsl --shutdown
```
- Installing and Uninstalling
   - install ubuntunu and setup username and password
``` powershell
wsl --install -d Ubuntu
```
   - uninstall ubuntu (when needed) using an elevated powershell 
``` powershell
wsl --list
wsl --unregister Ubuntu
wsl --list
```
- Update Ubuntu to the latest
``` bash
sudo apt update
sudo apt upgrade 
```

## Basic commands for navigating and interacting with WSL

### 1. Listing Distributions

To view a list of installed WSL distributions, use the following command:

```bash
wsl --list
```

This command will display the names of the installed distributions.

### 2. Launching a Distribution

To start a specific WSL distribution, use the following command:

```bash
wsl -d <DistributionName>
```

Replace `<DistributionName>` with the name of the distribution you want to launch.

### 3. Navigating the File System

In WSL, you can navigate the file system using standard Linux commands. For example:

- Change directory: `cd /path/to/directory`
- List files: `ls`
- List files with detailed information: `ls -l`
- Move or rename a file or directory: `mv old_name new_name`
- Copy files or directories: `cp source destination`
- Remove files or directories: `rm file_name` or `rm -r directory_name`

### 4. Windows File System Integration

Accessing Windows files from WSL is possible through the `/mnt` directory. For example:

```bash
cd /mnt/c/Users/YourUsername/Documents
```

Replace `YourUsername` with your actual Windows username.

### 5. Installing Packages

Use the package manager `apt` to install packages. For example:

```bash
sudo apt update
sudo apt install <package_name>
```

Replace `<package_name>` with the name of the package you want to install.

### 6. Running Command-Line Tools

You can run command-line tools and scripts in WSL just like you would in a native Linux environment.

```bash
./your_script.sh
```

### 7. Exiting WSL

To exit the WSL terminal, you can use the `exit` command.

```bash
exit
```

These are some basic commands to help you get started with navigating and interacting with WSL. Explore more commands and functionalities as you become familiar with the Linux environment within WSL.

## Installing Linux apps that supports GUI 

- Install Nautilus - Nautilus, also known as GNOME Files, is the file manager for the GNOME desktop. (Similar to Windows File Explorer).
``` bash
sudo apt install nautilus -y
# To launch Nautilus
nautilus
```
- Install VLC - VLC is a free and open source cross-platform multimedia player and framework that plays most multimedia files.
``` bash
sudo apt install vlc -y
# To launch VLC
vlc
```
- Install Gnome Text Editor - Gnome Text Editor is the default text editor of the GNOME desktop environment.
``` bash
sudo apt install gnome-text-editor -y
# To launch gnome editor and open your bash rc file
gnome-text-editor ~/.bashrc
```
- Install GIMP - GIMP is a free and open-source raster graphics editor used for image manipulation and image editing, free-form drawing, transcoding between different image file formats, and more specialized tasks.
``` bash
sudo apt install gimp -y
# To launch gimp
gimp
```

- Install X11 apps - X11 is the Linux windowing system and this is a miscellaneous collection of apps and tools that ship with it, such as the xclock, xcalc calculator, xclipboard for cut and paste, xev for event testing, etc. See the [x.org](https://www.x.org/wiki/UserDocumentation/GettingStarted/) docs for more info.
``` bash
sudo apt install x11-apps -y
#To launch, enter the name of the tool you would like to use. For example:

xcalc
xclock
xeyes
```
- Install Google Chrome for Linux
``` bash
# Change directories into the temp folder:
cd /tmp
# Use wget to download it: 
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
# Get the current stable version: 
sudo dpkg -i google-chrome-stable_current_amd64.deb
# Fix the package:
sudo apt install --fix-broken -y
# Configure the package:
sudo dpkg -i google-chrome-stable_current_amd64.deb
# To launch, enter:
google-chrome
```

## File System Integration

### 1. Accessing Windows Files from WSL

WSL provides seamless integration with the Windows file system. The `/mnt` directory is the gateway to accessing Windows files from within WSL. Here's how you can navigate and work with Windows files:

- Accessing your Windows user directory:

  ```bash
  cd /mnt/c/Users/YourUsername
  ```

  Replace `YourUsername` with your actual Windows username.

- Transferring files between Windows and WSL:

  You can copy files from Windows to WSL or vice versa using standard Linux commands like `cp`, `mv`, or `rsync`.

- Utilizing Windows paths:

  You can use Windows-style paths directly in WSL commands. For example:

  ```bash
  cat "C:\path\to\file.txt"
  ```

### 2. File Permissions and Ownership in WSL

WSL inherits the file permissions and ownership model from the Linux environment. Here are some key concepts:

- **Permissions:**

  Use the `chmod` command to change file permissions. For example:

  ```bash
  chmod +x script.sh
  ```

  This command grants execute permission to the script.

- **Ownership:**

  Use the `chown` command to change the ownership of a file. For example:

  ```bash
  chown user:group file.txt
  ```

  Replace `user` and `group` with the desired owner and group.

- **Sudo and Permissions:**

  Administrative tasks may require elevated privileges. Use `sudo` to execute commands as a superuser:

  ```bash
  sudo command
  ```

  You may be prompted to enter your password.

- **Default File Permissions:**

  Newly created files and directories inherit permissions from the parent directory. The `umask` command can adjust default permissions:

  ```bash
  umask 022
  ```

  This sets default permissions to allow read and execute for the owner and read for others.

Understanding and managing file permissions and ownership is crucial for maintaining a secure and organized WSL environment.