# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/3YDnKwD.jpg)

## Introduction

I've constructed a miniature honeynet in Azure, collecting log sources from various resources into a Log Analytics workspace. Microsoft Sentinel uses this data to build attack maps, trigger alerts, and create incidents. I measured security metrics in the insecure environment for 24 hours, applied security controls to harden the environment, measured metrics for another 24 hours, and now present the results below. These metrics include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel


## High Level Steps and Methodology
1. **_Create the honeynet:_** Build the environment within Azure including all virtual machines, storage accounts, key vaults, and security groups.
2. **_Logging and Monitoring:_** Create a Log Analystics work space to both ingest logs from the virtual machines, databases, and Azure network as well as feed those logs to Microsoft Sentinel which is the SIEM used for our SOC.
3. *Create Analystics Rules in Sentinel basd of off KQL Queries to trigger alerts.*
4. **_Environment observation:_** Observe the logs results and alerts triggered from a 24 hour exposure to both manufactured and live real traffic within our honeynet, establishing a baseline prior to hardening.
5. **_Incident Response and Remediation:_** Work fully all incidents triggered in our SIEM, investigate incidents, and remidiate Tru Positives.
6. *Harden Security Posture based on the observations of the insecure environment as well as recommendations from Sentinel and NIST 800-53.*
   


Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/302o12F.jpg)

Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/5dmnmpa.png)



# Attack Maps Before Hardening / Security Controls
## NSG Allowed Inbound Malicious Flows
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/reBkQwK.png)<br>
## Linux Syslog Auth Failures
![Linux Syslog Auth Failures](https://i.imgur.com/nbtZCT4.png)<br>
## Windows RDP/SMB Auth Failures
![Windows RDP/SMB Auth Failures](https://i.imgur.com/PogkqCa.png)<br>

## Metrics Before Hardening / Security Controls
For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use of Private Endpoints.

The following table shows the metrics we measured in our insecure environment for a 24 hour period:
Start Time 9/6/2023, 11:39:07 AM
Stop Time 9/7/2023, 11:39:07 AM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 106656
| Syslog                   | 34270
| SecurityAlert            | 13
| SecurityIncident         | 329
| AzureNetworkAnalytics_CL | 2296

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls
For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of allowing only the IP Address of my Admin workstation, and all other resources were protected by their built-in firewalls as well as utilizing Private Endpoints on the Key Vault and Storage Accounts, making them only accessible from within the Virtual Network.

The following table shows the metrics we measured in our environment for another 24 hours, after having applied the security controls:
Start Time 9/12/2023, 9:56:28 AM
Stop Time	9/13/2023, 9:56:28 AM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9543
| Syslog                   | 40295
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion


