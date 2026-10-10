# Attack Timeline

This timeline summarizes the network activity reconstructed during the Warzone 1 investigation.

## 1. Initial IDS Alert

The SOC received the following alert:

```text
ET MALWARE MirrorBlast CnC Activity M3
```

The alert involved:

```text
Source:      172[.]16[.]1[.]102
Destination: 169[.]239[.]128[.]11
```

## 2. Threat Intelligence Pivot

Threat-intelligence review associated the destination infrastructure with:

```text
Threat group: TA505
Malware family: MirrorBlast
Domain: fidufagios[.]com
```

VirusTotal showed that the majority communicating-file type associated with the domain was:

```text
Windows Installer
```

## 3. Suspicious Web Traffic

HTTP analysis identified the following unusual user-agent:

```text
REBOL View 2.7.8.3.1
```

This stood out from common browser user-agents and aligned with the malicious activity being investigated.

## 4. Additional Infrastructure

The compromised host also communicated with:

```text
185[.]10[.]68[.]235
192[.]36[.]27[.]92
```

## 5. Malware Downloads

The two additional hosts delivered:

```text
185[.]10[.]68[.]235 -> filter.msi
192[.]36[.]27[.]92 -> 10opd3r_load.msi
```

## 6. Secondary Payloads

Inspection of the first installer revealed references to:

```text
C:\ProgramData\001\arab.bin
C:\ProgramData\001\arab.exe
```

Inspection of the second installer revealed references to:

```text
C:\ProgramData\Local\Google\rebol-view-278-3-1.exe
C:\ProgramData\Local\Google\exemple.rb
```

## 7. Final Assessment

The combination of malicious infrastructure, TA505 attribution, MirrorBlast C2 traffic, unusual HTTP behavior, and secondary payload delivery confirmed the alert as a true positive.
