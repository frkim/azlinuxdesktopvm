# Walkthrough: Setup a Linux Desktop VM on Azure
This page explains how to setup a Mate desktop on Azure Linux VM.

  * Linux distro: Ubuntu
  * Version: From 18.04 LTS to 20 LTS
  * Desktop UI: Linux Mate

> I prefer Linux MATE desktop environment compared to GNOME 3 for performance reason in particular with remote display(less animations and chrome).

> Update (03/26 2021): you can provision an [Ubuntu Linux version 20.04](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/canonical.0001-com-ubuntu-server-focal?tab=Overview&modalAppId=canonical.0001-com-ubuntu-server-focal&signInModalType=1&ctaType=1). In that case, just go to Step #6 Intall Mate Desktop

## Step #0 - Provision a Linux Ubuntu VM on Azure 
[Quickstart: Create a Linux virtual machine in the Azure portal](https://docs.microsoft.com/fr-fr/azure/virtual-machines/linux/quick-create-portal)

## Step #1 - Version

check the current version of Linux

```bash
lsb_release -a
```

## Step #2 - Upgrade all installed packages
Type the following apt command to upgrade the installed packages:

```bash
sudo apt update
sudo apt list --upgradable
sudo apt upgrade


sudo reboot
```

## Step #3 - Remove all unused old kernels
Run the following to remove the unused old kernels:

```bash
sudo apt --purge autoremove
```

Make sure you install update-manager-core package
After updating Ubuntu server, run the commands below to install update-manager-core if it is not already installed.

```bash
sudo apt install update-manager-core
```

## Step #4 - Upgrade to latest LTS
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


## Step 5 - Verification
Check the Linux version:

```bash
lsb_release -a
```

## Step #6 - Intall Mate Desktop
See [How to Install MATE Desktop in Ubuntu Linux](https://itsfoss.com/install-mate-desktop-ubuntu/)
> Choose LightDM instead of GDM3

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install ubuntu-mate-desktop
```

## Step #7 - Install RDP

```bash
sudo apt-get -q=2 -y install xrdp
sudo service xrdp restart
```

## Step#8 - Linux Software Components
### Docker
Docker Install Commands : 
```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common	
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
```

ignore the error about public key could not be validated. 
```
sudo apt update
sudo apt install docker-ce
sudo systemctl status docker
sudo usermod -aG docker ${USER}   #run docker as SUDO without typing sudo all the time, make sure to log out and login again to SSH 
```

### Azure CLI
>Ubuntu 20.04 (Focal Fossa) and 20.10 (Groovy Gorilla) include an azure-cli package with version 2.0.81 provided by the universe repository. This package is outdated and not recommended. If this package is installed, remove the package before continuing by running the command sudo apt remove azure-cli -y && sudo apt autoremove -y.

```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

### VS Code
```
sudo snap install --classic code # or code-insiders
```
More information here: [Visual Studio Code on Linux - Installation](https://code.visualstudio.com/docs/setup/linux)

### Git
```
sudo apt install git-all
```

### kubectl Setup
```
az aks install-cli
```

### Helm
```
curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
sudo apt-get install apt-transport-https --yes
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

## Step #9 - Client Software

[MobaXTerm](https://mobaxterm.mobatek.net/) is a powerfull remote access software

## Step #10 - Misc
On Linux VM configuration, add network port rule to open the RDP protocol with a minimal IP range (use your own public IP instead of 'Any')
[Add RDP rule](https://docs.microsoft.com/en-us/troubleshoot/azure/virtual-machines/troubleshoot-rdp-nsg-problem)

For a more convinient access add a Static IP to the VM
[Add IP addresses](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-network-interface-addresses#add-ip-addresses)
Then instead of using the IP address which could change at every desallocation you'll able to use a dns name like 'mylinuxworkstation.westeurope.cloudapp.azure.com'

