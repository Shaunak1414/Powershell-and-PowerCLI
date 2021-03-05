# Powershell-and-PowerCLI Installation
Guide to install PowerShell Core and PowerCLI 10 on Ubuntu 20.04 to connect to a VMware vCenter or ESXi host.

## Installing Powershell via Package Repository
* Open Terminal and Update the list of packages

`sudo apt-get update`
* Install pre-requisite packages.

`sudo apt-get install -y wget apt-transport-https software-properties-common`
* Download the Microsoft repository GPG keys

`wget -q https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb`
* Register the Microsoft repository GPG keys

`sudo dpkg -i packages-microsoft-prod.deb`
* Update the list of products

`sudo apt-get update`
* Enable the "universe" repositories

`sudo add-apt-repository universe`
* Install PowerShell

`sudo apt-get install -y powershell`
* Start PowerShell

`pwsh`

## Installing PowerCLI 10
To install the PowerCLI 10, you just need to open the PowerShell with the pwsh command and run a install-module:

* For the system - you need higher permissions of course

`Install-Module -Name VMware.PowerCLI`

* For the current logged in user

`Install-Module -Name VMware.PowerCLI -Scope CurrentUser`

The installation takes a while and you need to agree to trust the PSGallery modules, as PowerCLI is one of them.

## Connecting to a vCenter:

`Connect-VIServer vcenter.dns.name -user -pass`

__Error :connect-VIserver the SSL connection cannot be established see inner exception__
`Set-PowerCLIConfiguration -InvalidCertificateAction Ignore`

### First Commands

`Get-VM`

`Get-VMHOST`

