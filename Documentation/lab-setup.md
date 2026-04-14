# Technical Setup & Infrastructure Detail

## 🏗️ Environment Architecture
The lab environment consists of a **Hybrid Cloud** model, linking a local physical virtualization host to Microsoft Azure.

### **On-Premises Components**
* **Hypervisor:** Hyper-V (Windows 11 Host)
* **Target Machine:** Windows Server 2022 
    * **Role:** Active Directory Domain Controller / File Server
    * **Connectivity:** Connected to Azure via **Azure Arc** for unified management.
* **Attacker Machine:** Kali Linux (Rolling Edition)
    * **Tools:** Hydra, Nmap.

### **Cloud Components (Azure)**
* **Log Analytics Workspace (LAW):** The central repository for all ingested security logs.
* **Microsoft Sentinel:** Utilized for SIEM/SOAR capabilities, detection rules, and incident management.
* **Azure Arc:** Used to onboard the physical Windows Server as a "Connected Machine," allowing for the installation of the Azure Monitor Agent (AMA).

## 🔧 Configuration Details
### **1. Log Collection**
Logs were collected from the Windows Server using the **Data Collection Rules (DCR)**. Specifically, `SecurityEvent` logs were filtered to prioritize:
* Event ID 4624 (Successful Logon)
* Event ID 4625 (Failed Logon)

### **2. SOAR Logic**
The Automation Playbook (Logic App) was configured with a **Managed Identity** to ensure secure, passwordless authentication to the Sentinel API. 
