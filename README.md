# Hybrid-SOC-Lab-with-automated-incident-response
# Obejective
To build a functional SOC environment that detects brute force attacks on physical endpoints and automates response actions using Microsoft Sentinel(SIEM) and Azure Logic Apps (SOAR)
# Technologies Used
    .SIEM/SOAR: Microsoft Sentinel
    .CLoud Infrastructure: Azure Log Analytics Workspace, Azure Arc
    .Automation: Azure Logic Apps
    .Endpoint: Windows Server 2022/Kali Linux(Attacker)
    .Tools: Hydra (Brute Force simulation), Kusto Query Language(KQL)
# Phase 1: Detection Engineering
    .Configured a custom KQL Analytics Rule to detect Event ID 4625 (Failed Login) in real-time
    .Logic: Flagged any single IP attempting more than 5 failed logins with 5 minutes
# Phase 2: SOAR Implementation
    .Create a Playbook (Logic App) triggered by the Sentinel Incident creation
    .Automated Workflow:
        1.Extract the Attacker's IP entity from the incident
        2.Enrich the incident by positng an Automated Comment containing the offending IP
        3.Future Scope: Integration with pfSense/Azure Firewall to block the IP
# Results and Evidence
