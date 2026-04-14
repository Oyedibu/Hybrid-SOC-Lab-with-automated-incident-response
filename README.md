# Hybrid SOC Home Lab: Detection and Response
This project demonstrates the implementation of a Hybrid SOC environment. I intergrated a local physical machine with Microsoft Sentinel using Azure Arc and built an automated SOAR playbook to identify and tag brute force attackers in real time
# Objective
To build a functional SOC environment that detects brute force attacks on physical endpoints and automates response actions using Microsoft Sentinel(SIEM) and Azure Logic Apps (SOAR)
'''mermaid
graph TD
    subgraph "On-Premises (Local Lab)"
        Kali["Kali Linux (Attacker)"] -- Brute Force / Hydra --> WS2022["Windows Server 2022"]
        WS2022 -- "Security Logs (Event 4625)" --> Arc["Azure Arc Agent"]
    end

    subgraph "Azure Cloud"
        Arc -- "Log Ingestion" --> LAW["Log Analytics Workspace"]
        LAW -- "KQL Query" --> Sentinel["Microsoft Sentinel"]
        
        Sentinel -- "Trigger Incident" --> LogicApp["Azure Logic App (SOAR)"]
        LogicApp -- "Add Comment with IP" --> Sentinel
    end

    style Kali fill:#f96,stroke:#333
    style Sentinel fill:#0078d4,color:#fff
    style LogicApp fill:#0078d4,color:#fff
'''
# Technologies Used
    .SIEM/SOAR: Microsoft Sentinel
    .CLoud Infrastructure: Azure Log Analytics Workspace, Azure Arc
    .Automation: Azure Logic Apps
    .Endpoint: Windows Server 2022 (Target)/Kali Linux(Attacker)
    .Tools: Hydra (Brute Force simulation), Kusto Query Language(KQL)
# Phase 1: Detection Engineering
    .Log Ingestion: Integratea physical Windows Server with Azure Sentinel using the Azure Arc and Log Analytics agent
    .KQL Development: COnfigured a custom Analytics Rule to monitor SecurityEvent logs for Event ID 4625(Failed Login)
    .Dectection Logic:
            kgl
            SecurityEvent
            | where EventID ==4625
            | summarize FailureCount = count() by IpAddress, TargetAccount, Computer
            | where FailureCount > 5
# Phase 2: SOAR Implementation
    .Trigger: Created a Logic App Playbook triggered automatically upon Sentinel Incident creation.
    .Workflow Logic:
        1.Get Incident: Retrieve full incident metadata.
        2.Entity Extraction: Utilized the Entities - Get IPs action to isolate the attacker's source address.
        3.Incident Enrichment: Automated the posting of a SOC comment back to the Sentinel incident, auditing the attacker's IP for immediate visibility.
# Challenges & Troubleshooting
    This phase involved overcoming several real-world configuration hurdles:
        .RBAC Permissions: Encountered "No Microsoft Sentinel permissions" errors. Resolved by granting the Sentinel Playbook Responder role to the Logic App within the specific Resource Group.
        .API Connection Policies: Attempted to use Gmail/Outlook connectors for notifications but pivoted to Internal Incident Comments bypass cross-tenant authentication restrictions (401 Unauthorized), keeping the workflow entirely within the SOC ecosystem.
        .Dynamic Data Mapping: Resolved an issue where IP addresses were returning empty brackets [] by refining the Entity Mapping in the Sentinel Analytics rule.
# Results & Evidence
    .Successful Attack Simulation: Hydra successfully triggered multiple alerts in Sentinel.
    .Automation Success: The Logic App successfully parsed the attack data and updated the incident comments within seconds of the breach detection.
# Future Scope
    .Active Blocking: Intergrating with pfSense or Azure Firwall APIs to automatically block the detected IP at the perimeter
    .Geographic Enrichment: Using IP geolocation APIs to tag the attacker's physical loaction in the incident logs
