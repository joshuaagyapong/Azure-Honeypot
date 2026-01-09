# Azure-Honeypot
# Building a SOC + Honeypot in Azure
![Honeynet and SOC](https://github.com/art-wiju/Cloud-SOC-And-Honeynet/assets/132944565/fbd21d13-f5b3-4e03-a956-e7550748028b)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel (SIEM) to build attack maps, trigger alerts, and create incidents in combination with KQL (Kusto Query Language). A combination of Cloud Security Implementations were implemented in this lab, such as Network Security Groups (NSGs), Microsoft Defender for Cloud, Microsoft Sentinel (SIEM). I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment in accordance to NIST 800-61 (Preparation, Detection & Analysis, Containment, Eradication & 
 Recovery and Post-Incident Activity), measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (1 windows)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machine had both the Network Security Group and built-in firewall wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Group were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by the built-in firewall as well as the Private Endpoint

## Attack Map Before Hardening / Security Controls
<img width="1708" height="796" alt="image" src="https://github.com/user-attachments/assets/32c3d045-7e7f-4c5b-a14a-f0a88b2dbe95" />

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 12/28/2025, 02:54 PM
Stop Time 12/29/2025, 02:54 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 2947
| Syslog                   | 134
| SecurityAlert            | 1
| SecurityIncident         | 215
| AzureNetworkAnalytics_CL | 843

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 1/29/2024, 5:30 PM
Stop Time	1/30/2024, 5:30 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 1364
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

An important note: The SecurityEvent processes left running are service accounts opening and closing OS processes. These are benign positives, expected and harmless. 

## Conclusion

In this project, a mini honeypot was constructed in Microsoft Azure and log sources were integrated into a Log Analytics Workspace. Kusto Query Language (KQL) was used to filter events from the Log Analytics Workspace. Microsoft Sentinel (SIEM) was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness. This lab was conducted in accordance with NIST 800-61 Computer Security Incident Handling Guide. 

It is worth noting that if the resources within the network were heavily utilized by the user, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
