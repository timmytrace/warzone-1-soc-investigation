# Indicators of Compromise

The following indicators were identified during the Warzone 1 investigation.

## Network Indicators

| Type | Indicator | Context |
|---|---|---|
| Internal host | `172[.]16[.]1[.]102` | Compromised endpoint |
| IPv4 | `169[.]239[.]128[.]11` | MirrorBlast C2 destination |
| IPv4 | `185[.]10[.]68[.]235` | Served `filter.msi` |
| IPv4 | `192[.]36[.]27[.]92` | Served `10opd3r_load.msi` |
| Domain | `fidufagios[.]com` | Associated malicious domain |

## Malware / File Indicators

```text
filter.msi
10opd3r_load.msi
arab.bin
arab.exe
rebol-view-278-3-1.exe
exemple.rb
```

## File Paths

```text
C:\ProgramData\001\arab.bin
C:\ProgramData\001\arab.exe
C:\ProgramData\Local\Google\rebol-view-278-3-1.exe
C:\ProgramData\Local\Google\exemple.rb
```

## Behavioral Indicator

Suspicious HTTP user-agent:

```text
REBOL View 2.7.8.3.1
```

## Attribution

```text
Threat group: TA505
Malware family: MirrorBlast
```
