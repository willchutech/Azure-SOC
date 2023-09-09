# Building a SOC + Honeynet in Azure (Live Traffic)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls


## Architecture After Hardening / Security Controls


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
![(Before)-windows-rdp-smb-auth-fail-24h](https://github.com/willchutech/Azure-SOC/assets/144466318/1212520f-c06f-4cf0-9517-9d648be9f86e)
![(Before)-syslog-ssh-auth-fail-24h](https://github.com/willchutech/Azure-SOC/assets/144466318/6a745245-f04f-4138-a511-458d60b54956)
![(Before)-mssql-auth-fail-24h](https://github.com/willchutech/Azure-SOC/assets/144466318/70532abe-b8ec-4579-8611-42dba0fad76d)
![(Before)-nsg-malicious-allowed-24h](https://github.com/willchutech/Azure-SOC/assets/144466318/652bc87e-26e2-400f-8aeb-2041f4446ab4)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:  
Start Time 9/4/2023 10:02:20  
Stop Time 9/5/2023 10:02:20  

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 84365
| Syslog                   | 3503
| SecurityAlert            | 6   
| SecurityIncident         | 316
| AzureNetworkAnalytics_CL | 21223

![Screenshot (1)](https://github.com/willchutech/Azure-SOC/assets/144466318/d8b28d40-5840-41d0-8886-68775994912d)


## Attack Maps Before Hardening / Security Controls

![(After)-windows-rdp-smb-auth-fail-24h](https://github.com/willchutech/Azure-SOC/assets/144466318/6dce9e56-d3cd-4aa5-9531-ad50877ec672)
```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 9/7/2023 1:03:41  
Stop Time	9/8/2023 1:03:41  

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 18137
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 1185

![harden-nsg](https://github.com/willchutech/Azure-SOC/assets/144466318/0299c25b-8759-44fa-be6b-84c963d306a2)
![(After)-harden1](https://github.com/willchutech/Azure-SOC/assets/144466318/51e33baa-a70b-4027-8ecf-8594fdaaa936)
![(After)-harden2](https://github.com/willchutech/Azure-SOC/assets/144466318/5c127de1-58cc-4821-b12b-235a117419ce)


## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
