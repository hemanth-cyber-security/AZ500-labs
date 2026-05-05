# 🔐 Azure Firewall Lab (AZ-500 Lab 03)

---

## 📌 Lab Scenario

You have been asked to install and configure **Azure Firewall** to control both **inbound and outbound traffic** as part of a secure network architecture.

### 🎯 Objectives

* Create a secure virtual network with:

  * Workload subnet
  * Jump host subnet
* Deploy virtual machines in each subnet
* Force all outbound traffic through the firewall using a custom route
* Allow only specific outbound access (`www.bing.com`)
* Allow DNS traffic for domain resolution

---

## 🏗️ Architecture

![Architecture](./screenshots/architecture.png)

---

## ⚙️ Step 1: Create Virtual Network

Create a Virtual Network:

* **Name**: `Lab03-VNet`
* **Address Space**: `10.0.0.0/16`

### Subnets:

1. `Workload-SN` → `10.0.2.0/24`
2. `Jump-SN` → `10.0.3.0/24`
3. `AzureFirewallSubnet` → `10.0.1.0/24` ⚠️ (Mandatory name)

---

## 🖥️ Step 2: Deploy Virtual Machines

### 🔹 Srv-Work (Private VM)

* Name: `Srv-Work`
* Subnet: `Workload-SN`
* Public IP: ❌ No
* NSG:

  * No inbound access from internet
  * Only internal access allowed

---

### 🔹 Srv-Jump (Public VM)

* Name: `Srv-Jump`
* Subnet: `Jump-SN`
* Public IP: ✅ Yes

#### NSG Rule:

* Allow RDP (3389) **only from your IP**

---

## 🔗 Step 3: Test Connectivity

[🔍 View Screenshot](./screenshots/connectivity.png)

![Connectivity](./screenshots/connectivity.png)

### Steps:

1. RDP from your laptop → **Srv-Jump (Public IP)**
2. From Srv-Jump → RDP to **Srv-Work (Private IP)**

📌 This validates:

* Internal communication works
* Jump host architecture is functioning

---

## 🔥 Step 4: Deploy Azure Firewall

[🔍 View Screenshot](./screenshots/firewall-overview.png)

![Firewall](./screenshots/firewall-overview.png)

### Configuration:

* Name: `Test-FW01`
* SKU: Standard
* VNet: `Lab03-VNet`
* Subnet: `AzureFirewallSubnet`
* Public IP: `TEST-FW-PIP`
* Firewall Management: **Classic rules**

📌 After deployment:

* Note the **Private IP of Firewall** (used in routing)

---

## 🛣️ Step 5: Create Route Table (Core Concept)

[🔍 View Screenshot](./screenshots/route-table.png)

![Route Table](./screenshots/route-table.png)

### Steps:

1. Create Route Table → `Firewall-route`
2. Associate with:

   * Subnet: `Workload-SN`

### Add Route:

* Destination: `0.0.0.0/0`
* Next Hop Type: `Virtual Appliance`
* Next Hop IP: **Firewall Private IP**

---

### 🧠 What this does

```id="flow1"
Srv-Work → Azure Firewall → Internet
```

📌 Forces all outbound traffic through firewall

---

## 🔐 Step 6: Configure Firewall Rules

---

### 🔹 Application Rule (Allow Bing)

[🔍 View Screenshot](./screenshots/app-rule.png)

![App Rule](./screenshots/app-rule.png)

* Source: `10.0.2.0/24`
* Protocols: HTTP, HTTPS
* Target: `www.bing.com`

---

### 🔹 Network Rule (DNS)

[🔍 View Screenshot](./screenshots/network-rule.png)

![Network Rule](./screenshots/network-rule.png)

* Source: `10.0.2.0/24`
* Destination:

  * `209.244.0.3`
  * `209.244.0.4`
* Port: `53`

---

## 🌐 Step 7: Configure DNS on Work VM

Set DNS manually on `Srv-Work`:

* `209.244.0.3`
* `209.244.0.4`

📌 Required for domain name resolution

---

## 🧪 Step 8: Test Firewall

---

### ✅ Allowed Traffic (Bing)

[🔍 View Screenshot](./screenshots/bing-allowed.png)

![Bing](./screenshots/bing-allowed.png)

* Successfully accessed:

```
https://www.bing.com
```

---

### ❌ Blocked Traffic (Microsoft)

[🔍 View Screenshot](./screenshots/microsoft-blocked.png)

![Blocked](./screenshots/microsoft-blocked.png)

* Access denied:

```
http://www.microsoft.com
```

* Error:

```
Action: Deny. Reason: No rule matched.
```

---

## 🔐 Key Security Concepts Learned

* ✔️ Forced tunneling using UDR
* ✔️ Default deny model
* ✔️ FQDN-based filtering
* ✔️ DNS dependency
* ✔️ Network segmentation
* ✔️ Jump host architecture

---

## ⚠️ Common Mistakes Avoided

* ❌ Using wrong subnet name for firewall
* ❌ Not associating route table
* ❌ Using firewall public IP instead of private
* ❌ Forgetting DNS rule
* ❌ Opening RDP to entire internet

---

## 🚀 Improvements (Real-World)

* Enable Firewall logging
* Integrate with Microsoft Sentinel
* Use Firewall Policy instead of classic rules
* Implement JIT VM access
* Restrict lateral movement using NSGs

---

## 🧹 Cleanup

```powershell id="cleanup1"
Remove-AzResourceGroup -Name "AZ500LAB08" -Force
```

---

## 📚 Conclusion

This lab demonstrates how Azure Firewall enforces **strict outbound control**, ensuring only trusted traffic is allowed and reducing the risk of:

* Data exfiltration
* Unauthorized access
* Malware communication

---

## 👨‍💻 Author

Hemanth – Cloud Security Learning Journey 🚀
