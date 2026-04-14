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
