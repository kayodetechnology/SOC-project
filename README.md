# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I established a miniature honeynet within the Azure cloud platform. I collected log data from diverse sources and directed it into a Log Analytics workspace. Microsoft Sentinel utilized this data to construct attack maps, initiate alerts, and generate incidents. For a comprehensive security assessment, I initially measured various security metrics within an insecure environment over a 24-hour period. Subsequently, I implemented specific security controls (NIST 800-53) to fortify the environment, followed by another 24-hour measurement of security metrics. The results of these measurements are presented below. The showcased metrics include:

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
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/6OCnR9P.jpeg)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/NB5dp26.jpeg)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/o2JOTlc.jpeg)<br>
![MSSQL Auth Failures](https://i.imgur.com/0BmVMDq.jpeg)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-02-27T17:17:56.9449504Z
Stop Time  2024-02-28T17:17:56.9449504Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 33039
| Syslog                   | 6721
| SecurityAlert            | 189
| SecurityIncident         | 1299
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, I established a miniature honeynet using Microsoft Azure, integrating log sources into a Log Analytics workspace. The orchestration of alerts and incidents was executed through Microsoft Sentinel, utilizing the ingested logs. Furthermore, metrics were initially gauged in an insecure environment before the enforcement of security controls. Subsequently, a follow-up measurement was conducted post-implementation of security measures. Notably, the application of security controls (NIST 800-53) resulted in a significant reduction in both security events and incidents, underscoring their efficacy.

It is essential to highlight that if the network resources experienced substantial utilization by regular users, there could have been a likelihood of generating more security events and alerts within the 24-hour period following the deployment of security controls.
