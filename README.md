# 🔐 Azure Security Labs – AZ-500

This repository contains hands-on labs completed as part of the official
**AZ-500: Microsoft Azure Security Technologies** learning path.

🔗 Lab Source: AZ-500 Azure Security Technologies Labs

---

# 📘 About AZ-500

The AZ-500 certification focuses on implementing and managing security in Azure, including:

* Identity & Access Management
* Platform Protection
* Data & Application Security
* Security Operations

These labs are designed to provide **real-world, hands-on experience** aligned with Azure Security Engineer responsibilities.

---

# 🎯 Repository Goal

* Practice Azure security concepts hands-on
* Understand real-world IAM and RBAC scenarios
* Build a **cloud security portfolio**
* Prepare for **AZ-500 certification & security roles**

---

# 🧪 Labs Progress

## ✅ Lab 01 – Role Based Access Control (RBAC)

### 🔹 Objective

Implement and manage **RBAC in Azure** by creating users, groups, and assigning access.

### 🔹 Tasks Performed

* Created users:

  * Joseph Price (Portal)
  * Isabel Garcia (PowerShell - Microsoft Graph)
  * Dylan Williams (Azure CLI)

* Created groups:

  * Senior Admins
  * Junior Admins
  * Service Desk

* Added users to groups using:

  * Azure Portal
  * PowerShell (Microsoft Graph API)
  * Azure CLI

---

### 🔹 Tools Used

* Azure Portal
* Microsoft Graph PowerShell
* Azure CLI

---

### 🔹 Key Concepts Learned

* Role-Based Access Control (RBAC)
* Microsoft Entra ID (Identity Management)
* Graph API & Permissions Model
* Difference between:

  * RBAC Roles vs Graph API Scopes
* User → Group → Role mapping

---

### 🔹 Security Perspective 🔐

This lab simulates real-world IAM scenarios:

* User creation → **Persistence**
* Group membership → **Access control**
* Role assignment → **Privilege escalation risk**

These activities are critical to monitor in:

* Microsoft Sentinel
* SIEM tools
* Identity Protection systems

---

# 📂 Repository Structure

```bash
azure-security-labs/
│
├── LAB1_RBAC.md
└── README.md
```

---

# 🚀 Upcoming Labs

| Lab    | Topic                   | Status      |
| ------ | ----------------------- | ----------- |
| Lab 01 | RBAC                    | ✅ Completed |
| Lab 02 | Network Security Groups | ⏳ Pending   |
| Lab 03 | Azure Firewall          | ⏳ Pending   |
| Lab 04 | ACR & AKS Security      | ⏳ Pending   |
| Lab 05 | Azure SQL Security      | ⏳ Pending   |

---

# 🧠 Skills Gained

* Identity & Access Management (IAM)
* Automation using PowerShell & CLI
* Understanding Azure Security Architecture
* Troubleshooting permission & authorization issues

---

# 📌 Notes

* All labs performed in a personal Azure lab environment
* Commands and scripts are documented for learning purposes
* Replace sensitive values before reuse

---

# 👨‍💻 Author

Hands-on learning journey towards becoming a **Cloud Security Engineer**
(Focused on Azure Security & SC-200 / AZ-500)

---

[1]: https://www.coursera.org/professional-certificates/microsoft-azure-security-engineer-associate?utm_source=chatgpt.com "Microsoft Azure Security Engineer Associate (AZ-500)"
