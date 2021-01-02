# Walkthrough to setup a Linux Desktop VM on Azure
This page explains how to setup a Linux VM on Azure with a desktop UI.

Linux distro: Ubuntu
Version: From 18.04 LTS to 20 LTS
Desktop UI: Linux Mate

> I prefer installing Linux MATE instead of GNOME 3 for performance reason in particular with remote display (less animations and chrome).

# Step#0 - Provision a Linux Ubuntu VM on Azure 
[Quickstart: Create a Linux virtual machine in the Azure portal](https://docs.microsoft.com/fr-fr/azure/virtual-machines/linux/quick-create-portal)

# Step#1 - Version

check the current version of Linux

```bash
lsb_release -a
```

# Step #2 - Upgrade all installed packages
Type the following apt command to upgrade the installed packages:

```bash
sudo apt update
sudo apt list --upgradable
sudo apt upgrade


sudo reboot
```

# Step #3 - Remove all unused old kernels
Run the following to remove the unused old kernels:

```bash
sudo apt --purge autoremove
```

Make sure you install update-manager-core package
After updating Ubuntu server, run the commands below to install update-manager-core if it is not already installed.

```bash
sudo apt install update-manager-core
```

Step #4 - Upgrade to latest LTS
Execute the following command:

```bash
sudo do-release-upgrade
```

> Please note if you may be greeted with the following message:
> Checking for a new Ubuntu release
> There is no development version of an LTS available.
> To upgrade to the latest non-LTS develoment release 
> set Prompt=normal in /etc/update-manager/release-upgrades.
> In that case, pass the -d option to get the latest supported release forcefully:

```bash
sudo do-release-upgrade -d
```


#Step 5 - Verification
Check the Linux version:

```bash
lsb_release -a
```

# Step #5 - Intall Mate Desktop
See [How to Install MATE Desktop in Ubuntu Linux](https://itsfoss.com/install-mate-desktop-ubuntu/)
> Choose LightDM instead of GDM3

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install ubuntu-mate-desktop
```

# Step #6 - Install RDP

```bash
sudo apt-get -q=2 -y install xrdp
sudo service xrdp restart
```
