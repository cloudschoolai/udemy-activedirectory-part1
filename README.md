# Active Directory Deep Dive: Windows Server Identity and Access Management
> **Official Everything as Code (EaC) Repository for the `cloudschool.ai` Enterprise Series**

Welcome to the official code, automation, and blueprint repository for the **Active Directory Deep Dive** course. This repository is designed around an **Everything as Code (EaC)** philosophy—meaning no manual portal clicking, no dead links, and zero guesswork. Every module in the course maps directly to version-controlled artifacts you can deploy in your local Linux KVM lab or enterprise production environment.



## 📂 Repository Structure

The repository is organized into four core directories to support your multi-node hybrid lab architecture (Ubuntu KVM host + Windows Server 2025 Desktop/Core + Windows Server 2028 Preview + Azure IaaS Cloud DC):

```text

├── blueprints/        # Architectural diagrams, network topologies, and conceptual markdown guides
├── runbooks/          # Step-by-step operational, deployment, and troubleshooting procedures
├── scripts/           # Automation assets: PowerShell (.ps1), Ansible playbooks (.yml), and Azure Bicep (.bicep)
└── prompts/           # Exclusive A.I.D.E.N. framework AI prompts and LLM operational toolkits

```


- /blueprints
  
Contains high-level conceptual documentation, logical network layouts, forest design specifications, and enterprise security segmentation models. Use these to understand the "Why" before touching the keyboard.

- /runbooks
  
Production-grade, step-by-step operational runbooks. Each runbook pairs with course lectures to guide you through tasks like Forest Genesis, AD-integrated DNS configuration, GPO precedence troubleshooting, and SSSD/Samba Linux client joining.

- /scripts
  
Your execution engine, containing:

Ansible (/scripts/ansible/): Playbooks to automatically configure your Ubuntu 26.04 host with QEMU, KVM, bridge networking, and VirtIO storage drivers.
  
Azure Bicep (/scripts/bicep/): Declarative infrastructure code to spin up your Azure IaaS Cloud Domain Controller as the 4th node in your lab.
  
PowerShell (/scripts/powershell/): Production-safe scripts for automated AD forest promotion, bulk user/group provisioning, and GPO validation (.ps1).

- /prompts
  
Home of the A.I.D.E.N. (Automated Infrastructure Directory Engineering Navigator) framework. Access curated system prompts and prompt-engineering toolkits designed to help system administrators safely leverage AI for log parsing, script generation, and rapid incident remediation.


## 🚀 Getting Started with Your Course Lab
To set up your environment to match the course curriculum:

### Clone the Repository:

Bash
git clone [https://github.com/cloudschoolai/udemy-activedirectory-part1.git](https://github.com/cloudschoolai/udemy-activedirectory-part1.git)
cd udemy-activedirectory-part1

### Provision Your KVM Host (Ubuntu 26.04):
Navigate to /scripts/ansible/ and run the host configuration playbook to prepare your hypervisor.

### Deploy the Hybrid Cloud Node:
Navigate to /scripts/bicep/ and deploy your Azure Domain Controller template via the Azure CLI (az deployment group create...).

### Setting Up the Ansible Runtime (For Ubuntu 26.04 KVM Host)
Ansible automates the configuration of your Ubuntu hypervisor host (installing KVM packages, setting up default bridge networks, and configuring storage pools).

Installation on Ubuntu 26.04:

```Bash
sudo apt update
sudo apt install software-properties-common -y
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virtinst -y
```
Validation: Run `ansible --version` and `virsh` uri to ensure local libvirt connectivity is active. Review the baseline playbook in the /ansible folder so you can mirror your host setup with a single command (ansible-playbook setup-kvm-host.yml).

### Setting Up the Azure Bicep & Azure CLI Runtime
Bicep provides declarative syntax to deploy your Azure IaaS Domain Controller VM without relying on the changing Azure Portal web interface.

Installation on Ubuntu 26.04:

```Bash
# Install Azure CLI
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

# Install Bicep CLI via Azure CLI
az bicep install
```
Validation & Usage: Log in with az login, create a resource group, and test your deployment:

```Bash
az group create --name rg-ad-lab-hybrid --location eastus
az deployment group create --resource-group rg-ad-lab-hybrid --template-file main.bicep
```

### Setting Up PowerShell 7 & DSC Runtime
PowerShell 7 is cross-platform, fast, and essential for managing Windows Server Core, AD DS modules, and Desired State Configuration.

Installation on Ubuntu 26.04 (for script authoring/testing):

```Bash
# Update package list and install prerequisites
sudo apt-get update
sudo apt-get install wget apt-transport-https software-properties-common -y

# Download the Microsoft repository GPG keys & register package
source /etc/os-release
wget -q https://packages.microsoft.com/config/ubuntu/$VERSION_ID/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb

# Install PowerShell
sudo apt-get update
sudo apt-get install powershell -y
```
Windows Server Management Modules: Inside PowerShell, ensure you have the necessary modules ready for your demos:

```PowerShell
Install-Module -Name ActiveDirectory -Force
Install-Module -Name PsDesiredStateConfiguration -Force
```

## 🛡️ License & Usage
  
This repository is provided as companion material for students enrolled in the cloudschool.ai Active Directory series on Udemy. All scripts and runbooks are provided "as-is" under the MIT License for educational and professional use.

Built with precision for modern infrastructure engineers.

