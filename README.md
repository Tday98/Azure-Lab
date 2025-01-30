# Azure-Lab SOC + Honeynet w/ Live Traffic

![Honeynet Overview](https://github.com/Tday98/Azure-Lab/assets/18738382/508be7eb-1fea-4c5b-9815-591b56474794)


## Overview
For this homelab I used Azure to create a honeynet with an Ubuntu Linux VM and Microsoft Windows 10 VM. I ingested logs from different resources within Azure into Log Analytics workspace. I used Microsoft Sentinel (SIEM) to produce attack maps, trigger alerts, and incidents. I gathered attack data over 24 hours in the insecure environment. Then I applied common security controls to harden the honeynet and repeated a 24 hour window to compare the data with the insecure environment. I used the data to create an attack map which showed attackers ip locations. I compared the two honeynets to show the improvements I made with the security controls.

### Logs Collected
- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Security Controls
![Prehardened](https://github.com/Tday98/Azure-Lab/assets/18738382/1c864972-ee6e-4ac5-b62e-644cf9f980e1)

## Architecture After Security Controls
![Posthardened](https://github.com/Tday98/Azure-Lab/assets/18738382/48c14da4-fca6-4f29-af16-d3ba5287d65e)

Architecture:
- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (1 Windows, 1 Ubuntu Linux)
- Log Analytics Workspace
- Azure Key Vault
- Blob Storage
- Microsoft Sentinel

The above architecture was left exposed to the public internet for 24 hours. The NSGs were left wide open and the built-in windows firewalls were disabled. All resources were deployed with public endpoints. Data was collected and sent to log analytics to be aggregated and then were viewed through Microsoft Sentinel (SIEM).

Hardening the environment was done by: 
1. Only allowing access to the VMs from my IPv4 address via inbound rules on the NSG.
2. Built-in firewalls were enabled as was the Azure cloud firewall.
3. A private VNet was established which created private endpoints for resources to be accessed by the Virtual Machines.

## Attack Maps Before Security Controls
![linux-ssh-auth-fail-pre](https://github.com/Tday98/Azure-Lab/assets/18738382/172655b7-d2da-4e15-9f62-01555ca2b59c)
![nsg-malicious-allowed-in-pre](https://github.com/Tday98/Azure-Lab/assets/18738382/4f8683e2-fd23-49c4-b8fd-f7294cf2fb90)
![sql-fail-pre](https://github.com/Tday98/Azure-Lab/assets/18738382/bfcf83cb-1618-4afe-9e1c-a4a9604b7b74)
![windows-rdp-auth-fail-pre](https://github.com/Tday98/Azure-Lab/assets/18738382/f7c1f7ec-6875-4a8a-b66c-74fce31178e5)

### Table For Prehardened Metrics
Start Time	2024-05-17 20:30
Stop Time	2024-05-18 20:30
| Type of Log   | Amount |
| -------- | ------- |
| Security Events (Windows VMs) | 57286 |
| Syslog (Linux VMs) | 11149 |
| SecurityAlert (Microsoft Defender for Cloud)   | 43 |
|  SecurityIncident (Sentinel Incidents) | 355 |
| NSG Inbound Malicious Flows Allowed | 2211 |

#### NOTE: All Map Queries For Posthardened Archtecture Returned NULL

### Table For Posthardened Metrics
Start Time	2024-05-18 23:00
Stop Time	2024-05-20 17:00
| Type of Log   | Amount |
| -------- | ------- |
| Security Events (Windows VMs) | 10750 |
| Syslog (Linux VMs) | 4541 |
| SecurityAlert (Microsoft Defender for Cloud)   | 0 |
|  SecurityIncident (Sentinel Incidents) | 0 |
| NSG Inbound Malicious Flows Allowed | 0 |

| Change after security environment |
| Type of Log   | Amount |
| -------- | ------- |
|Security Events (Windows VMs)|	-81.23%|
|Syslog (Linux VMs)|-59.27%|
|SecurityAlert (Microsoft Defender for Cloud)	|-100.00%|
|Security Incident (Sentinel Incidents)	|-100.00%|
|NSG Inbound Malicious Flows Allowed|-100.00%|

### Security Metrics
![Screenshot 2024-05-20 170137](https://github.com/Tday98/Azure-Lab/assets/18738382/9cd59164-f3ac-4a3e-a49d-a8b0e7b1fe49)
Total secure score was raised from 45% to 77% post hardening. Also, NIST SP 800 53 R5 compliance standard was used as a guideline to help improve security.

## Conclusion
I used Azure to build a honeypot/honeynet architecture. I used Sentinel to raise incidences and used Log Analytics to query logs from all the resources. The system was ran 24 hours with no security and 24 hours with security and a comparison was made. I used Microsoft Cloud Security Benchmark and NIST SP 800 53 R5 compliance standards to improve security of the system.

### Credits
Credits to Leveld and Josh Madakor for putting together this homelab for students like myself to follow and learn a lot about cloud cybersecurity.
https://www.leveldcareers.com/products/leveld-cyber-security-masterclass
