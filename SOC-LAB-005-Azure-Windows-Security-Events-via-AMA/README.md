# LAB-005 – Collecting Windows Security Events using Azure Monitor Agent (AMA)

## Overview

This lab demonstrates how to configure Azure Monitor Agent (AMA) together with a Data Collection Rule (DCR) to collect Windows Security Events from an Azure Virtual Machine and send them to an Azure Log Analytics Workspace.

After configuring the monitoring pipeline, Kusto Query Language (KQL) queries were executed to validate that Windows Security Events were successfully ingested and made available for investigation in Microsoft Sentinel.

This implementation represents a common operational scenario performed by SOC Analysts and Cloud Security Engineers when onboarding Windows servers into a SIEM environment.

---

# Objectives

- Deploy Azure Monitor Agent (AMA)
- Configure a Data Collection Rule (DCR)
- Associate the DCR with an Azure Virtual Machine
- Collect Windows Security Events
- Validate log ingestion in Azure Log Analytics
- Execute KQL queries to verify collected events
- Confirm agent connectivity using Heartbeat telemetry

---

# Technologies Used

- Microsoft Azure
- Azure Virtual Machine
- Azure Monitor Agent (AMA)
- Azure Monitor
- Azure Log Analytics Workspace
- Data Collection Rules (DCR)
- Microsoft Sentinel
- Windows Server 2022
- Kusto Query Language (KQL)

---

# Environment

| Component | Value |
|------------|-------|
| Virtual Machine | SOC-LAB005-VM |
| Operating System | Windows Server 2022 |
| Azure Region | Sweden Central |
| Resource Group | RG-SOC-LABS |
| Log Analytics Workspace | LAW-SOC-LABS |
| Monitoring Agent | Azure Monitor Agent (AMA) |

---

# Architecture

```text
Windows Server 2022 VM
        │
        ▼
Azure Monitor Agent (AMA)
        │
        ▼
Data Collection Rule (DCR)
        │
        ▼
Azure Log Analytics Workspace
        │
        ▼
Microsoft Sentinel
        │
        ▼
Kusto Query Language (KQL)
        │
        ▼
Security Monitoring & Investigation
```

---

# Implementation

## Step 1 – Provision the Virtual Machine

A Windows Server 2022 virtual machine was deployed in Microsoft Azure to serve as the monitored endpoint.

---

## Step 2 – Install Azure Monitor Agent

The Azure Monitor Agent (AMA) extension was installed to enable telemetry collection and communication with Azure Monitor.

---

## Step 3 – Configure the Data Collection Rule

A Data Collection Rule (DCR) was created to collect Windows Security Event Logs.

The DCR was configured to collect:

- Windows Security Events

The rule was then associated with the virtual machine.

---

## Step 4 – Validate Log Collection

After the association was completed, Windows Security Events began to populate the **SecurityEvent** table inside Azure Log Analytics.

KQL queries were executed to validate the successful ingestion of security logs.

---

# Validation

## Windows Security Events

### Query

```kql
SecurityEvent
| take 10
```

### Result

The query returned Windows Security Events collected from the monitored virtual machine, confirming successful communication between Azure Monitor Agent and Azure Log Analytics.

### Evidence

![Windows Security Events](images/securityevent-take10.png)

---

## Event ID Summary

### Query

```kql
SecurityEvent
| summarize count() by EventID
| order by EventID asc
```

### Result

The returned Event IDs confirm that Windows Security auditing events are being successfully collected and indexed.

### Evidence

![Security Event ID Summary](images/securityevent-eventid-summary.png)

---

## Azure Monitor Heartbeat

### Query

```kql
Heartbeat
| summarize count() by Computer
```

### Result

Heartbeat telemetry confirms that the Azure Monitor Agent is actively communicating with Azure Log Analytics.

### Evidence

![Heartbeat Summary](images/heartbeat-summary-by-computer.png)

---

## Data Collection Rule

The Data Collection Rule was successfully created and associated with the monitored virtual machine.

### DCR Overview

![DCR Overview](images/dcr-overview.png)

### Associated Resource

![Associated Resource](images/dcr-associated-resource.png)

### Windows Event Collection

![Windows Event Collection](images/windows-event-collection.png)

---

# KQL Queries Used

### Display Security Events

```kql
SecurityEvent
| take 10
```

### Display Event Distribution

```kql
SecurityEvent
| summarize count() by EventID
| order by EventID asc
```

### Verify Agent Connectivity

```kql
Heartbeat
| summarize count() by Computer
```

---

# Troubleshooting Journey

During the implementation, Windows Security Events were not initially appearing in the **SecurityEvent** table.

The investigation included validation of:

- Azure Monitor Agent installation
- Heartbeat telemetry
- Data Collection Rule configuration
- Virtual Machine association
- Windows Security Event generation
- Microsoft Sentinel connector configuration

The root cause was identified as the absence of a Data Collection Rule created through the **Windows Security Events via AMA** connector.

After creating the connector-managed DCR and associating it with the virtual machine, Windows Security Events were successfully ingested into Azure Log Analytics.

This troubleshooting process provided valuable hands-on experience with Azure-native monitoring components and log ingestion workflows.

---

# Skills Demonstrated

- Azure Administration
- Azure Monitor
- Azure Monitor Agent (AMA)
- Data Collection Rules (DCR)
- Azure Log Analytics
- Microsoft Sentinel
- Windows Security Monitoring
- Windows Event Logging
- Kusto Query Language (KQL)
- Cloud Monitoring
- Cloud Security
- SOC Operations
- Security Event Analysis
- Troubleshooting

---

# Key Learning Outcomes

Throughout this lab, the complete Azure-native monitoring pipeline was implemented and validated.

Key concepts practiced include:

- Azure Monitor Agent deployment
- Data Collection Rule configuration
- Windows Security Event collection
- Log Analytics validation
- KQL querying
- Microsoft Sentinel integration
- Troubleshooting Azure Monitor data ingestion

These activities closely resemble operational tasks performed by SOC Analysts and Cloud Security Engineers responsible for enterprise monitoring environments.

---

# References

- Microsoft Learn – Azure Monitor Agent
- Microsoft Learn – Data Collection Rules
- Microsoft Learn – Azure Log Analytics
- Microsoft Learn – Microsoft Sentinel
- Microsoft Learn – Kusto Query Language (KQL)

---

# Conclusion

This lab successfully demonstrated the deployment of Azure Monitor Agent (AMA), the configuration of a Data Collection Rule (DCR), and the collection of Windows Security Events into Azure Log Analytics.

The monitoring pipeline was validated using KQL queries and Heartbeat telemetry, confirming successful event ingestion into Microsoft Sentinel.

Beyond the technical implementation, this lab also reinforced troubleshooting methodologies used to diagnose and resolve log ingestion issues in Azure-native security monitoring environments.
