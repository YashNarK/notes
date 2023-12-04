# Windows Subsystem for Linux

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
