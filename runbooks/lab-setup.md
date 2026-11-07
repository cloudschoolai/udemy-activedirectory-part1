# Course Lab Setup Guide

This runbook provides validated, error-free installation instructions to set up your local development and virtualization environment for the **Active Directory Deep Dive** course. For course lab introduction and high-level Github instructions, review the **[repo Readme file](https://github.com/cloudschoolai/udemy-activedirectory-part1/blob/main/README.md)**.

---

## 1. Prerequisites & System Requirements
* **Host Operating System:** Ubuntu 26.04 LTS (Clean installation recommended)
* **Hardware:** Minimum 16GB RAM, 4+ CPU cores, VT-x/AMD-V virtualization enabled in BIOS.
* **Cloud Account:** A free-tier or developer Azure subscription (for the hybrid cloud DC node).

---

## 2. Setting Up the Ansible Runtime & KVM Host
Ansible automates the configuration of your Ubuntu hypervisor host (installing QEMU/KVM packages, setting up default bridge networks, and configuring storage pools).

### Installation on Ubuntu 26.04:
```bash
sudo apt update
sudo apt install software-properties-common -y
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virtinst -y
```

### Validation
Verify that your local libvirt daemon is active and running:
```bash
ansible --version
virsh uri
```
(Expected output should return qemu:///system). Review the baseline **playbook in the /scripts/ansible/ folder** to mirror your host setup with a single command:

```Bash
ansible-playbook setup-kvm-host.yml
```

## 3. Setting Up the Azure Bicep & Azure CLI Runtime
Bicep provides declarative syntax to deploy your Azure IaaS Domain Controller VM without relying on the changing Azure Portal web interface.

Installation on Ubuntu 26.04:
```Bash
# Install Azure CLI
curl -sL [https://aka.ms/InstallAzureCLIDeb](https://aka.ms/InstallAzureCLIDeb) | sudo bash

# Install Bicep CLI via Azure CLI
az bicep install
```
Validation & Usage:
Log in to your Azure subscription, create a dedicated resource group, and test your deployment:

```Bash
az login
az group create --name rg-ad-lab-hybrid --location eastus
az deployment group create --resource-group rg-ad-lab-hybrid --template-file ../scripts/bicep/main.bicep
```
## 4. Setting Up PowerShell 7 & DSC Runtime
PowerShell 7 is cross-platform, fast, and essential for managing Windows Server Core, AD DS modules, and Desired State Configuration.

Installation on Ubuntu 26.04 (for script authoring/testing):
```Bash
# Update package list and install prerequisites
sudo apt-get update
sudo apt-get install wget apt-transport-https software-properties-common -y

# Download the Microsoft repository GPG keys & register package
source /etc/os-release
wget -q [https://packages.microsoft.com/config/ubuntu/$VERSION_ID/packages-microsoft-prod.deb](https://packages.microsoft.com/config/ubuntu/$VERSION_ID/packages-microsoft-prod.deb)
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb

# Install PowerShell
sudo apt-get update
sudo apt-get install powershell -y
```

Installing PowerShell 7 on Windows Server
Unlike Linux, on Windows Server you can install PowerShell 7 quickly using winget (the Windows Package Manager) or via a single PowerShell web-install script.

- Option A: Using Windows Package Manager (winget)
Run this in an elevated PowerShell session (Administrator):

```PowerShell
winget install --id Microsoft.PowerShell --source winget
```
- Option B: Using the Standard MSI Installer via PowerShell
Run this script block in an elevated PowerShell session to download and silently install the latest PowerShell 7 MSI:

```PowerShell
$ProgressPreference = 'SilentlyContinue'
Invoke-WebRequest -Uri "https://github.com/PowerShell/PowerShell/releases/download/v7.4.x/PowerShell-7.4.x-win-x64.msi" -OutFile "$env:TEMP\powershell.msi"
Start-Process msiexec.exe -ArgumentList '/package', "$env:TEMP\powershell.msi", '/quiet', 'ADD_EXPLORER_CONTEXT_MENU_OPENPOWERSHELL=1', 'ENABLE_PSREMOTING=1' -Wait
Remove-Item "$env:TEMP\powershell.msi"
```

## 5. Installing AD RSAT and Management Consoles (MMC / AD Features) on Windows Server
Depending on whether you are running Windows Server Desktop Experience or Windows Server Core in your lab, the commands to install Active Directory management tools differ slightly.

Windows Server Management Modules:
Inside your PowerShell session, first ensure you have the necessary administration modules ready for course demos:

```PowerShell
Install-Module -Name ActiveDirectory -Force
Install-Module -Name PsDesiredStateConfiguration -Force
```

For Windows Server Desktop Experience (GUI):
Run this in an elevated PowerShell session to install the full Active Directory module, Group Policy Management Console, and classic MMC RSAT tools:

```PowerShell
Install-WindowsFeature RSAT-AD-PowerShell, RSAT-AD-Tools, GPMC -IncludeAllSubFeature
```
For Windows Server Core (Headless):
Server Core does not support graphical MMC consoles (like dsa.msc or gpmc.msc), but it fully supports remote management and PowerShell administration modules. Run this in an elevated PowerShell session:

```PowerShell
Install-WindowsFeature RSAT-AD-PowerShell
```
Verification & Module Imports
Once installed, verify your PowerShell version and load your administration modules by running:

```PowerShell
# Check PowerShell Version
$PSVersionTable

# Import Active Directory Module (if not auto-loaded)
Import-Module ActiveDirectory

# Test AD Module Availability
Get-Command -Module ActiveDirectory
```

---
You are now fully prepared to execute the Everything as Code workflow for Course Part 1!