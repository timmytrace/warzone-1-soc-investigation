# Wireshark Analysis

This document records the Wireshark workflow used to investigate the Warzone 1 PCAP.

## 1. Inspect the Flagged C2 IP

Filter:

```text
ip.addr == 169.239.128.11
```

This revealed communication between:

```text
172.16.1.102 -> 169.239.128.11
```

The internal source was therefore identified as the compromised host.

## 2. Inspect HTTP Traffic to the Flagged IP

Filter:

```text
http && ip.addr == 169.239.128.11
```

For an HTTP request packet, expand:

```text
Hypertext Transfer Protocol
```

The user-agent observed was:

```text
REBOL View 2.7.8.3.1
```

## 3. Find Other External Hosts Contacted by the Compromised Endpoint

Filter:

```text
ip.addr == 172.16.1.102 && http
```

Reviewing destination addresses revealed two additional external hosts:

```text
185.10.68.235
192.36.27.92
```

Another useful view is:

```text
Statistics -> Conversations -> IPv4
```

This helps summarize IP-to-IP communication patterns.

## 4. Identify the First Download

Filter:

```text
ip.addr == 185.10.68.235 && http.request
```

Inspect the HTTP request URI. The downloaded file was:

```text
filter.msi
```

## 5. Identify the Second Download

Filter:

```text
ip.addr == 192.36.27.92 && http.request
```

Inspect the HTTP request URI. The downloaded file was:

```text
10opd3r_load.msi
```

## 6. Search Specifically for MSI Downloads

Filter:

```text
http.request.uri contains ".msi"
```

This is a quick way to identify MSI installer requests in the capture.

## 7. Inspect the `filter.msi` Conversation

Filter:

```text
ip.addr == 185.10.68.235
```

Select the relevant TCP/HTTP packet and use:

```text
Right-click -> Follow -> TCP Stream
```

Search the stream for strings such as:

```text
ProgramData
arab
```

The installer content referenced:

```text
C:\ProgramData\001\arab.bin
C:\ProgramData\001\arab.exe
```

## 8. Inspect the `10opd3r_load.msi` Conversation

Filter:

```text
ip.addr == 192.36.27.92
```

Follow the TCP stream and search for:

```text
ProgramData
Google
rebol
```

The installer content referenced:

```text
C:\ProgramData\Local\Google\rebol-view-278-3-1.exe
C:\ProgramData\Local\Google\exemple.rb
```

## 9. Export HTTP Objects

Wireshark can reconstruct transferred files with:

```text
File -> Export Objects -> HTTP
```

This view can be used to locate transferred objects such as:

```text
filter.msi
10opd3r_load.msi
```

Malware samples should only be handled in an isolated, authorized analysis environment.

## Analyst Takeaway

The strongest evidence came from correlating network behavior rather than relying on a single alert. The IDS signature provided the starting point, Wireshark exposed the C2 communication and payload delivery chain, and threat intelligence provided attribution context.
