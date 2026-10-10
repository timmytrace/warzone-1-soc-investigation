# Warzone 1 – SOC Investigation

This repository documents a hands-on Tier 1 SOC investigation based on the TryHackMe **Warzone 1** room.

The scenario begins with an IDS/IPS alert for potentially malicious traffic and suspected command-and-control activity. The goal was to triage the alert, validate whether it was a true positive, identify the infrastructure involved, trace downloaded payloads, and reconstruct the attack using Wireshark and VirusTotal.

> This repository is for defensive-security learning in an authorized lab environment. The original PCAP is not redistributed here.

## Scenario

Role: Tier 1 Security Analyst at an MSSP

Initial alert:

```text
ET MALWARE MirrorBlast CnC Activity M3
```

The compromised internal host was observed communicating with infrastructure associated with TA505 and the MirrorBlast malware family.

## Key Findings

| Artifact | Finding |
|---|---|
| Source host | `172[.]16[.]1[.]102` |
| C2 destination | `169[.]239[.]128[.]11` |
| Threat group | `TA505` |
| Malware family | `MirrorBlast` |
| Associated domain | `fidufagios[.]com` |
| Majority communicating file type | `Windows Installer` |
| User-Agent | `REBOL View 2.7.8.3.1` |
| Additional infrastructure | `185[.]10[.]68[.]235`, `192[.]36[.]27[.]92` |
| Downloaded files | `filter.msi`, `10opd3r_load.msi` |

## Attack Reconstruction

The internal host `172.16.1.102` communicated with `169.239.128.11`, which was associated with MirrorBlast command-and-control activity.

The investigation then identified two additional external systems:

```text
185[.]10[.]68[.]235
192[.]36[.]27[.]92
```

These hosts delivered:

```text
filter.msi
10opd3r_load.msi
```

The first installer referenced the following dropped files:

```text
C:\ProgramData\001\arab.bin
C:\ProgramData\001\arab.exe
```

The second installer referenced:

```text
C:\ProgramData\Local\Google\rebol-view-278-3-1.exe
C:\ProgramData\Local\Google\exemple.rb
```

## Analyst Assessment

This alert was determined to be a **true positive**.

Evidence supporting this conclusion included:

- outbound communication from an internal host to known malicious infrastructure
- TA505 attribution in threat-intelligence sources
- MirrorBlast-related command-and-control traffic
- an unusual REBOL user-agent
- multiple MSI payload downloads from external infrastructure
- installer content referencing secondary payloads and scripts written to disk

## Repository Structure

```text
warzone-1-soc-investigation/
├── README.md
├── investigation/
│   ├── timeline.md
│   ├── indicators-of-compromise.md
│   └── wireshark-analysis.md
└── filters/
    └── wireshark-filters.txt
```

## Skills Demonstrated

`SOC Analysis` · `Wireshark` · `PCAP Analysis` · `Network Forensics` · `Threat Intelligence` · `VirusTotal` · `Malware Triage` · `Incident Triage` · `IOC Analysis` · `Command and Control`
