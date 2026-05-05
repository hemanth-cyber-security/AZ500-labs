# 🔐 Azure Firewall Lab (AZ-500 Lab 03)

---

## 📌 Overview

This lab demonstrates how to deploy and configure Azure Firewall to control outbound traffic using:

* Application Rules (FQDN filtering)
* Network Rules (DNS)
* User Defined Routes (UDR)

---

## 🏗️ Architecture

[🔍 View Full Image](./images/architecture.png)

![Architecture](./images/architecture.png)

---

## ⚙️ Step 1: Create Virtual Network

* **Name**: Lab03-VNet
* **Address Space**: 10.0.0.0/16

### Subnets:

* Workload-SN → 10.0.2.0/24
* Jump-SN → 10.0.3.0/24
* AzureFirewallSubnet → 10.0.1.0/24 ⚠️ (Required)

---

## 🖥️ Step 2: Deploy Virtual Machines

### 🔹 Srv-Work (Private VM)

* Subnet: Workload-SN
* Public IP: ❌ No
* NSG: No inbound from internet

---

### 🔹 Srv-Jump (Public VM)

* Subnet: Jump-SN
* Public IP: ✅ Yes

#### NSG Rule:

* Allow RDP (3389) only from your IP

---

## 🔗 Step 3: Connectivity Test

[🔍 View Screenshot](./images/connectivity.png)

![Connectivity](./images/connectivity.png)

### Test:

* RDP → Srv-Jump (Public IP)
* RDP → Srv-Work (Private IP from Jump VM)

---

## 🔥 Step 4: Deploy Azure Firewall

[🔍 View Screenshot](./images/firewall-overview.png)

![Firewall](./images/firewall-overview.png)

* Firewall Name: Test-FW01
* SKU: Standard
* Subnet: AzureFirewallSubnet
* Note the **Private IP**

---

## 🛣️ Step 5: Route Table (UDR)

[🔍 View Screenshot](./images/route-table.png)

![Route Table](./images/route-table.png)

### Configuration:

* Route: 0.0.0.0/0
* Next Hop: Virtual Appliance
* Next Hop IP: Firewall Private IP
* Associate with: Workload-SN

---

## 🔐 Step 6: Firewall Rules

### 🔹 Application Rule (Allow Bing)

[🔍 View Screenshot](./images/app-rule.png)

![Application Rule](./images/app-rule.png)

* Source: 10.0.2.0/24
* Protocol: HTTP/HTTPS
* Target: [www.bing.com](http://www.bing.com)

---

### 🔹 Network Rule (DNS)

[🔍 View Screenshot](./images/network-rule.png)

![Network Rule](./images/network-rule.png)

* Source: 10.0.2.0/24
* Destination: 209.244.0.3, 209.244.0.4
* Port: 53

---

## 🌐 Step 7: Configure DNS

* Set DNS on Srv-Work:

  * 209.244.0.3
  * 209.244.0.4

---

## 🧪 Step 8: Test Results

### ✅ Allowed (Bing)

[🔍 View Screenshot](./images/bing-allowed.png)

![Bing Allowed](./images/bing-allowed.png)

---

### ❌ Blocked (Microsoft)

[🔍 View Screenshot](./images/microsoft-blocked.png)

![Blocked](./images/microsoft-blocked.png)

```text
Action: Deny. Reason: No rule matched.
```
 
---

## 🔐 Key Learnings

* Forced tunneling using UDR
* Default deny model
* FQDN filtering
* DNS dependency
* Network segmentation

---

## 🧹 Cleanup

```powershell
Remove-AzResourceGroup -Name "AZ500LAB08" -Force
```

---

## 👨‍💻 Author

Hemanth – Cloud Security Learning Journey 🚀
