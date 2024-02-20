# Building a Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I built a mini honeynet in Azure and ingested log sources from various resources into the Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, applied some security controls to harden the environment, measured metrics for another 24 hours, then gathered the results shown below. In this project, I deployed 2 extremely vulnerable virtual machines(1 Windows 10 & 1 Linux Ubuntu) by turning off their firewalls, which essentially left them wide open. While the firewalls were off, I let the machines run for over 24 hours on the internet. This created a plethora of security incidents and gave me the opportunity to apply my skills in a live environment, by learning about NIST 800-61 framework. Afterwards, I applied some of the recommended security controls, in order to secure my environment and then measured the same metrics 24 hours later. The results over the course of the two days are shown below:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Unsecured Azure Environment](https://github.com/James-Jeudy/Honeynet-Azure/assets/160562010/1bfdb22d-6e09-493e-836d-cd08b602a6f4)


## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

Here are the listed components that were found in the architecture of the honeypot in Azure:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open. Microsoft Entra ID was used to create users, assign them roles, and login attempts were generated from the same users. Kusto Query Language(KQL) was used to gather logs inside of the Log Analytics workspace, and was used to create the attack world maps. 

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](![NSG-Malicious-Allowed-In before](https://github.com/James-Jeudy/Honeynet-Azure/assets/160562010/82947c3f-b953-492e-9a73-629452100632)
)<br>
![Linux Syslog Auth Failures](![Syslos-SSH-Auth-Fail Before](https://github.com/James-Jeudy/Honeynet-Azure/assets/160562010/6de95a14-2f00-4723-9218-e8200d2c9795)
)<br>
![Windows RDP/SMB Auth Failures](![Windows-RDP-SMB-Auth-fail json before](https://github.com/James-Jeudy/Honeynet-Azure/assets/160562010/eba40a16-ed12-4187-8394-90e219873037)
)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-02-14 02:48:56
Stop Time 2024-02-14 02:48:56

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 132838
| Syslog                   | 23188
| SecurityAlert            | 7
| SecurityIncident         | 270
| AzureNetworkAnalytics_CL | 103

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-02-19 02:30:09
Stop Time	2024-02-20 2:30:09

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 17357
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Percentage of Improvement After Hardening Environment

| Metric                   | Change
| ------------------------ | -----
| SecurityEvent            | -86.93%
| Syslog                   | -85.71%
| SecurityAlert            | -100%
| SecurityIncident         | -100%
| AzureNetworkAnalytics_CL | -100%

## Conclusion

In this project, a honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was utilized to trigger alerts and create incidents based on the ingested logs from Logs Workspace. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing some security measures like NIST 800-53, in particular Boundary Protection. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their high effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls. Either way, this was a wonderful project that helped me improve my skillset and also provided continuous learning after just passing my Security Blue Team Level One Exam!
