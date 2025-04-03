# Install the OS

# Raspbian

Download and install the Raspberry Pi Imager

[Operating system images - Raspberry Pi](https://www.raspberrypi.com/software/operating-systems/)

When the Imager is open, Click “Choose OS”.

![[rpiImager.png|Screen Shot 2022-03-09 at 10.16.07 am.png]]

Select Raspberry Pi OS (Other).

![[rpiImagerChooseOS.png|Screen Shot 2022-03-09 at 10.16.30 am.png]]

In the list, choose Raspberry PI OS (64-bit).

![[rpiImagerChooseOS64bit.png|Screen Shot 2022-03-09 at 10.16.58 am.png]]

Click Choose Storage. 

Choose the attached SD Card.

![[rpiImagerChooseStorage.png|Screen Shot 2022-03-09 at 10.17.36 am.png]]

Click Write.

# Ubuntu

When the Imager is open, Click “Choose OS”.

![[rpiImagerOS.png]]

Choose General Purpose OS.

![[rpiImagerGeneralPurposeOS.png|Screen Shot 2022-03-09 at 10.21.30 am.png]]

Choose Ubuntu Desktop. 

Make sure it’s not the Server version.

![[rpiImagerUbuntuDesktop.png|Screen Shot 2022-03-09 at 10.19.36 am.png]]

Click Choose Storage. 

Choose the attached SD Card.

![[rpiImagerChooseStorage.png]]

# Update the System

> [!important] Take a screenshot of these instructions below for VET Competency ICTICT213 - 2.4


First set the time to the current time

`sudo date -s "Thu 14 Mar 2024 1300"`

Run the following commands at the terminal. Press Enter after each command.

```bash
cd Documents
git clone http://github.com/Lake-Tuggeranong-College/RPi
cd RPi
chmod +x rpi-setup.sh
sudo ./rpi-setup.sh
```



# Configure the time service

> [!important] Take a screenshot of these instructions below for VET Competency ICTICT213 - 3.1


After updating the system, open the terminal.

enter the following commands

```bash
sudo apt install ntp ntpdate
sudo service ntp stop
sudo ntpdate 203.62.5.5
sudo service ntp start
```

# Installing Software

> [!important] Take a screenshot of this instructions below for VET Competency ICTICT213 - 2.5

## Install VS Code extensions

within Visual Studio Code, install the following extensions:

- PlatformIO IDE
- PHP Server
- Database Client JDBC (by Weijan Chen)
- MySQL (by Weijan Chen)
