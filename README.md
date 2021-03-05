# Powershell-and-PowerCLI
Guide to install PowerShell Core and PowerCLI 10 on Ubuntu 20.04 to connect to a VMware vCenter or ESXi host.

## Installation via Package Repository
* Update the list of packages
`sudo apt-get update`
* Install pre-requisite packages.
sudo apt-get install -y wget apt-transport-https software-properties-common
* Download the Microsoft repository GPG keys
wget -q https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb
* Register the Microsoft repository GPG keys
sudo dpkg -i packages-microsoft-prod.deb
* Update the list of products
sudo apt-get update
* Enable the "universe" repositories
sudo add-apt-repository universe
* Install PowerShell
sudo apt-get install -y powershell
* Start PowerShell
pwsh
