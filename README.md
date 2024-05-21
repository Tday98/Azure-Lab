# Azure-Lab SOC + Honeynet w/ Live Traffic

![Honeynet Overview](https://github.com/Tday98/Azure-Lab/assets/18738382/508be7eb-1fea-4c5b-9815-591b56474794)


## Overview
For this homelab I used Azure to create a honeynet with an Ubuntu Linux VM and Microsoft Windows 10 VM. I ingested logs from different resources within Azure into Log Analytics workspace. I used Microsoft Sentinel (SIEM) to produce attack maps, trigger alerts, and incidents. I gathered attack data over 24 hours in the insecure environment. Then I applied common security controls to harden the honeynet and repeated a 24 hour window to compare the data with the insecure environment. I used the data to create an attack map which showed attackers ip locations. I compared the two honeynets to show the improvements I made with the security controls.

### Logs collected
- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)
