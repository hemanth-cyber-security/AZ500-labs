# Azure IAM Labs – RBAC (Hands-on)

This repository documents hands-on labs performed to understand **Azure Entra ID (IAM)** concepts using:

* Azure Portal
* PowerShell (Microsoft Graph)
* Azure CLI

---

#  Exercise 1 – Azure Portal

## Objective

Create a **Senior Admins** group and add user **Joseph Price**.

## Steps

1. Create user **Joseph Price** in Azure Portal
2. Create a group named **Senior Admins**
3. Add Joseph Price to the group

---

#  Exercise 2 – PowerShell (Microsoft Graph)

## Objective

Create a **Junior Admins** group and add **Isabel Garcia** using PowerShell.

---

##  Step 1: Install Microsoft Graph Module

```powershell
Install-Module Microsoft.Graph -Scope CurrentUser -Force
Import-Module Microsoft.Graph.Users
```

---

##  Step 2: Connect to Entra ID

```powershell
Connect-MgGraph -Scopes "User.ReadWrite.All"
```

---

##  Step 3: Create User (Isabel Garcia)

```powershell
$passwordProfile = @{
    Password = "Temp@123456"
    ForceChangePasswordNextSignIn = $true
}

New-MgUser `
    -DisplayName "Isabel Garcia" `
    -UserPrincipalName "isabel.garcia@az500demolabgmail.onmicrosoft.com" `
    -MailNickname "isabelgarcia" `
    -AccountEnabled `
    -PasswordProfile $passwordProfile
```

---

##  Step 4: Create Group

```powershell
Connect-MgGraph -Scopes "Group.ReadWrite.All","Directory.ReadWrite.All"

New-MgGroup `
    -DisplayName "Junior Admins group" `
    -MailEnabled:$false `
    -MailNickname "JuniorAdmins" `
    -SecurityEnabled:$true
```

---

##  Step 5: Get User and Group IDs

```powershell
Get-MgUser -Filter "DisplayName eq 'Isabel Garcia'" | Select Id
Get-MgGroup -Filter "DisplayName eq 'Junior Admins group'" | Select Id
```

---

##  Step 6: Add User to Group

```powershell
New-MgGroupMember `
    -GroupId "<GROUP_ID>" `
    -DirectoryObjectId "<USER_ID>"
```

---

#  Exercise 3 – Azure CLI

## Objective

Create a **Service Desk** group and add **Dylan Williams**.

---

##  Step 1: Create User

```bash
az ad user create \
  --display-name "Dylan Williams" \
  --user-principal-name "dylan.williams@az500demolabgmail.onmicrosoft.com" \
  --password "Temp123@" \
  --mail-nickname "dylanwilliams"
```

---

##  Step 2: List Users (Verification)

```bash
az ad user list --query "[].{Name:displayName, ID:id}" -o table
```

---

##  Step 3: Create Group

```bash
az ad group create \
  --display-name "Service Desk" \
  --mail-nickname "servicedesk"
```

---

##  Step 4: Verify Group

```bash
az ad group list --query "[].{Name:displayName, Id:id}" -o table
```

---

##  Step 5: Add User to Group

```bash
az ad group member add \
  --group "<GROUP_ID>" \
  --member-id "<USER_ID>"
```

---

#  Key Learnings

* Azure IAM uses **RBAC (Role-Based Access Control)**
* Users and Groups are core identity objects
* PowerShell and CLI interact with **Microsoft Graph API**
* Permissions require:

  * Proper **Azure Role**
  * Correct **Graph API Scopes**
* Always use **Object IDs**, not display names

---

#  Security Perspective

These operations are critical from a security standpoint:

* Creating users → Persistence
* Creating groups → Access structuring
* Adding members → Privilege escalation

Monitoring these actions is essential in:

* Microsoft Sentinel
* SIEM solutions
* Identity protection systems

---

#  Tools Used

* Azure Portal
* Microsoft Graph PowerShell
* Azure CLI

---

#  Notes

* Ensure correct permissions (Global Admin / Group Admin)
* Replace placeholders like `<GROUP_ID>` and `<USER_ID>` with actual values
* Use strong passwords as per Azure policy

---

# 📎 Author

Hands-on lab practice for Azure IAM & Cloud Security learning.
