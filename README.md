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


## 🚀 Quick Start
Clone the Repository:

```Bash
git clone [https://github.com/cloudschoolai/udemy-activedirectory-part1.git](https://github.com/cloudschoolai/udemy-activedirectory-part1.git)
cd udemy-activedirectory-part1
```
Configure Your Lab Environment:
  
Head over to the **Lab Setup Guide** for full, step-by-step instructions on provisioning your Ubuntu KVM host, Azure Bicep environment, and PowerShell runtimes.

## 🛡️ License & Usage
This repository is provided as companion material for students enrolled in the cloudschool.ai Active Directory series on Udemy. All scripts and runbooks are provided "as-is" under the MIT License for educational and professional use.

Built with precision for modern infrastructure engineers.