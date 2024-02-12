# Building a Honeynet in Azure

## Introduction

In this project I construct a honeynet within Azure, integrating log sources from different resources into a Log Analytic workspace. This workspace is utilized by Mircosft Sentinel for generating attack maps, initiating alerts, and generating incidents. For 24 hours I conducted security metric assessments in an unsecured environment. I strengthened the environment by applying some security controls and reassessing for another 24 hours, then showing the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/E7YaUKP.png)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/A6UoJfV.png)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

In the 'BEFORE' metrics, all the resources were set up and made accessible and exposed to the internet. The Virtual Machines didn't have restrictions on their Network Security Groups or built-in firewalls, and all other resources were openly available online without using Private Endpoints.

In the 'AFTER' metrics, the Network Security Groups were made much stricter. All traffic was blocked except for connections from my admin workstation. Additionally, built-in firewalls within Azure were used and Private Endpoints to secure all other resources.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/wpwk8BP.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/mLfgA6u.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-02-9 21:33:51
Stop Time 2024-02-10 21:33:51

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 21966
| Syslog                   | 50365
| SecurityAlert            | 6
| SecurityIncident         | 314
| AzureNetworkAnalytics_CL | 38368

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24-hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-02-10 22:41
Stop Time	2024-02-11 22:41

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 6098
| Syslog                   | 12
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, I successfully established a mini honeynet within the Microsoft Azure environment, integrating log sources into a Log Analytics workspace. Utilizing Microsoft Sentinel, I was able to efficiently generate alerts and create incidents from the collected logs. A key aspect of the project involved evaluating metrics in an unsecured environment, both pre and post the application of security measures. The results were significant in that there was a marked decrease in the number of security events and incidents was observed following the implementation of these controls, underscoring their effectiveness.

However, it's important to consider that in scenarios where network resources are extensively used by regular users, the likelihood of observing a higher volume of security events and alerts could increase, particularly in the 24-hour window subsequent to the application of the security controls.
