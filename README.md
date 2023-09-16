# Building a SOC and Honeynet in Azure with Live Traffic

![image](https://github.com/willchutech/Azure-SOC/assets/144466318/28d3ca2b-5400-4f2c-b258-afdc55c20a37)

## Introduction

In this project, I set up a small honeynet within the Azure platform. My primary objective was to capture and analyze logs from multiple sources, which I then consolidated in a Log Analytics workspace. I deployed Microsoft Sentinel to harness these logs, allowing me to create attack maps, establish alert triggers, and generate incident reports. Azure Sentinel enabled me to measure the metrics of an insecure environment over a 24-hour period. After completing this phase, I implemented security measures to bolster the virtual environment's defenses. Lastly, I conducted another 24-hour metric measurement phase, and I'm presenting the results of these efforts below. The metrics I examined include:"

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Utilized Technologies, Azure Components, and Adhered Regulations

1. **Establish Azure Virtual Network (VNet):**
   - Create and configure the Azure Virtual Network infrastructure.
2. **Implement Azure Network Security Groups (NSG):**
   - Set up and manage Azure NSGs to control network traffic.
3. **Deploy Virtual Machines (2 Windows VMs, 1 Linux VM):**
   - Provision two Windows VMs and one Linux VM to meet project requirements.
4. **Utilize Log Analytics Workspace with Kusto Query Language (KQL) Queries:**
   - Leverage a Log Analytics Workspace to perform data analysis using Kusto Query Language (KQL) queries.
5. **Secure Secrets with Azure Key Vault:**
   - Employ Azure Key Vault for the secure management of sensitive information.
6. **Manage Data with Azure Storage Account:**
   - Create and configure an Azure Storage Account for efficient data storage.
7. **Implement Microsoft Sentinel (SIEM):**
   - Deploy Microsoft Sentinel for Security Information and Event Management (SIEM) capabilities.
8. **Enhance Cloud Security with Microsoft Defender:**
   - Utilize Microsoft Defender for Cloud to protect and secure cloud resources.
9. **Enable Remote Access with Windows Remote Desktop:**
   - Set up Windows Remote Desktop for remote access to virtual machines.
10. **Utilize Command Line Interface (CLI) for System Management:**
    - Leverage the Command Line Interface (CLI) for effective system management tasks.
11. **Automate and Manage Configuration with PowerShell:**
    - Harness PowerShell for automation and configuration management.
12. **Implement NIST SP 800-53 Revision 5 Security Controls:**
    - Adhere to the security controls outlined in NIST SP 800-53 Revision 5.
13. **Follow NIST SP 800-61 Revision 2 Incident Handling Guidance:**
    - Abide by the guidelines provided in NIST SP 800-61 Revision 2 for incident handling.

## Architecture Before Hardening / Security Controls

In the initial "BEFORE" phase of the project, I deployed a virtual environment and intentionally made it accessible to the public to draw the attention of potential malicious actors. My goal in this stage was to attract these bad actors and observe their attack patterns. To achieve this, I deployed a Windows virtual machine, a SQL database and a Linux server, of which had their network security groups (NSGs) configured to Allow All traffic. To make the environment even more enticing to potential attackers, I set up a storage account and a key vault with public endpoints that were visible on the open internet. Throughout this stage, I closely monitored the unsecured environment using Microsoft Sentinel, which analyzed logs aggregated by the Log Analytics workspace.

## Architecture After Hardening / Security Controls

In the "AFTER" phase of the project, I focused on strengthening the environment and implementing security measures to align with NIST SP 800-53 Rev4 SC-7(3) Access Points. These security enhancements included:

- Network Security Groups (NSGs): I hardened the NSGs by blocking all inbound and outbound traffic, except for designated public IP addresses that needed access to the virtual machines. This approach ensured that only authorized traffic from trusted sources could reach the virtual machines.

- Built-in Firewalls: I configured Azure's built-in firewalls on the virtual machines to restrict unauthorized access and safeguard the resources from malicious connections. This step involved fine-tuning the firewall rules based on the specific services and responsibilities of each VM, effectively reducing the attack surface available to potential bad actors.

- Private Endpoints: To enhance the security of Azure Key Vault and Storage Containers, I replaced Public Endpoints with Private Endpoints. This change limited access to these sensitive resources solely to the virtual network and prevented exposure to the public internet.


## Attack Maps Before Hardening / Security Controls

This attack map illustrates all the unauthorized access attempts made by malicious actors targeting the Windows virtual machine.
![(Before)-windows-rdp-smb-auth-fail-24h](https://github.com/willchutech/Azure-SOC/assets/144466318/1212520f-c06f-4cf0-9517-9d648be9f86e)
![(Before)-syslog-ssh-auth-fail-24h](https://github.com/willchutech/Azure-SOC/assets/144466318/6a745245-f04f-4138-a511-458d60b54956)
![(Before)-mssql-auth-fail-24h](https://github.com/willchutech/Azure-SOC/assets/144466318/70532abe-b8ec-4579-8611-42dba0fad76d)

This attack map displays the inbound traffic permitted by a Network Security Group where all traffic is allowed.
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


## Attack Maps After Hardening / Security Controls

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

In this project, I created a compact yet highly efficient honeynet in Microsoft Azure and seamlessly integrated various log sources into a Log Analytics workspace. I meticulously configured Microsoft Sentinel to promptly generate alerts and incident reports based on the log data. Furthermore, I conducted a comprehensive analysis of security metrics in the unprotected environment, both before and after the implementation of rigorous security measures.

Notably, following the introduction of these enhanced security controls, there was a remarkable improvement in the security landscape. Specifically, there was a 96% reduction in Windows Security Events, a staggering 99% decrease in Linux Events, and a complete elimination of security alerts, incidents, and malicious inbound network traffic, marking a 100% reduction in these concerning security issues.
