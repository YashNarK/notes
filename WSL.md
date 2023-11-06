# Windows Subsystem for Linux

## Installing Ubuntu & Ubuntu Desktop

1) install ubuntunu and setup username and password
``` bash
wsl --install -d Ubuntu
```
2) Install X410 from microsoft store for X server
3) Install Ubuntu Desktop
``` bash
sudo apt update
sudp apt upgrade 
sudo apt install tasksel
sudo tasksel install ubuntu-desktop
sudo apt install xfce4
sudo apt install xfce4-session
```
4) Set display environment
``` bash
export DISPLAY=:0
```
5) Start X server by starting X410
6) Install & start d-bus
``` bash
sudo apt install dbus
sudo service dbus start
```
7) Set environment variables
``` bash
export DBUS_SESSION_BUS_ADDRESS=/dev/null
export $(dbus-launch)
``` 
8) Start Ubuntu desktop 
``` bash
startxfce4
```