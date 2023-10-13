# Building a Cloud SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I constructed a miniature honeynet within Azure and channeled logs from multiple resources into a Log Analytics workspace. This data is subsequently utilized by Microsoft Sentinel to develop attack maps, initiate alerts, and establish incidents. I gauged specific security metrics in a vulnerable environment over a 24-hour period, then implemented security measures to fortify the environment. After assessing metrics for an additional 24 hours, the results are presented below. The metrics displayed include:

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


For the "BEFORE" metrics, all resources were initially deployed with direct internet exposure. The Virtual Machines had their Network Security Groups and inherent firewalls fully unguarded. Furthermore, all other resources were deployed with public-facing endpoints, meaning there was no utilization of Private Endpoints.

For the "AFTER" metrics, we fortified the Network Security Groups by restricting all traffic, save for my admin workstation. Additionally, all other resources were safeguarded using their intrinsic firewalls in conjunction with Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our vulnerable environment for 24 hours:
Start Time 2023-10-10 20:09:07
Stop Time 2023-10-10 20:09:07

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 42933
| Syslog                   | 1732
| SecurityAlert            | 5
| SecurityIncident         | 156
| AzureNetworkAnalytics_CL | 3687

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-10-11 22:27
Stop Time	2023-10-12 22:27

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8129
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
