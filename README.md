# Cloud SOC (SIEM + Honeynet Live Traffic) Project in Azure
![Soc-siem](https://github.com/user-attachments/assets/64356823-6a3a-408c-aded-6e80491103b1)


## Introduction

In this project, I build a honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)
## Regulations and Guidelines
- NIST SP 800-53 Revision 5 for Security Controls
- NIST SP 800-61 Revision 2 for Incident Handling Guidance

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://github.com/user-attachments/assets/514fc2a3-c46b-4219-9112-84ed329748c3)



## Architecture After Hardening / Security Controls
![Architecture Diagram](https://github.com/user-attachments/assets/db7e1673-064a-438b-8f3f-2f4c2be97270)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/FkS85QW.jpg)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/cj3BEec.jpg)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/Z26Q64m.jpg)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-07-10 16:02:39
Stop Time 2024-07-11 16:02:39

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 18352
| Syslog                   | 11633
| SecurityAlert            | 11
| SecurityIncident         | 205
| AzureNetworkAnalytics_CL | 1696

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-07-12 10:44:29
Stop Time	2024-07-13 10:44:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 7725
| Syslog                   | 2
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, I created a honeynet in Microsoft Azure and integrated log sources into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and generate incidents based on the ingested logs. Metrics were collected from the insecure environment before applying security controls and again afterward. The data revealed a significant reduction in security events and incidents post-implementation, underscoring the effectiveness of the security measures.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of security controls.
