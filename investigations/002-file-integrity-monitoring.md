# Detection 002 — File Integrity Monitoring (FIM)

**Platform:** Wazuh Cloud  
**Endpoint:** macOS (Apple Silicon)  
**Detection Rule:** 550  
**Severity:** Level 7  
**Status:** Successfully detected  
**Date:** 25 October 2026  
**MITRE ATT&CK:** T1565.001 — Stored Data Manipulation

---

## 1. Project Overview

This project demonstrates the implementation and testing of File Integrity Monitoring (FIM) using Wazuh SIEM.

The objective was to detect modifications to a monitored configuration file and investigate the resulting security alert.

File Integrity Monitoring is an important cybersecurity capability because unauthorised changes to system files, application configurations or sensitive data may indicate malicious activity.

This exercise simulated a file modification in a controlled environment to validate Wazuh's detection capabilities.

## 2. Lab Environment

| Component | Technology |
|---|---|
| SIEM | Wazuh Cloud |
| Endpoint | Apple Silicon MacBook |
| Operating System | macOS |
| Wazuh Agent | v4.14.8 |
| Monitoring Method | Scheduled File Integrity Monitoring |
| Scan Frequency | 300 seconds during testing |
| Test Directory | `/Users/ashwin/soc-lab-files` |
| Test File | `config.txt` |

## 3. Configuration

First, I created a dedicated testing directory and configuration file using Terminal.

```bash
mkdir -p ~/soc-lab-files
echo "Version 1 - Original configuration" > ~/soc-lab-files/config.txt
```

I then modified the Wazuh agent configuration file located at:

`/Library/Ossec/etc/ossec.conf`

The following settings were configured within the existing `<syscheck>` section:

```xml
<disabled>no</disabled>
<directories check_all="yes">/Users/ashwin/soc-lab-files</directories>
<frequency>300</frequency>
<scan_on_start>yes</scan_on_start>
```

These settings enabled scheduled monitoring of the test directory, with integrity checks performed approximately every five minutes.

Following the configuration changes, I restarted the Wazuh agent:

```bash
sudo /Library/Ossec/bin/wazuh-control restart
```

Agent logs confirmed that the test directory was being monitored and that File Integrity Monitoring scans were completing successfully.

## 4. Detection Simulation

To simulate a file modification, I appended additional text to the monitored configuration file.

```bash
echo "Version 2 - Configuration modified" >> ~/soc-lab-files/config.txt
```

This changed the file contents, file size, modification timestamp and cryptographic hashes.

The purpose of this test was to determine whether Wazuh would identify the change by comparing the file against its previously established integrity baseline.

## 5. Detection Results

Wazuh successfully generated a file integrity alert.

| Attribute | Result |
|---|---|
| Agent | macos-soc-lab |
| Rule ID | 550 |
| Rule Description | Integrity checksum changed |
| Severity | Level 7 |
| Event Type | Modified |
| Detection Method | Scheduled |
| Detection Time | 25 October 2026, 19:14:36 |
| Original File Size | 70 bytes |
| Modified File Size | 101 bytes |

The following attributes were identified as changed:

- File size
- Modification timestamp
- MD5 checksum
- SHA-1 checksum
- SHA-256 checksum

### SHA-256 Hash Comparison

**Original SHA-256:**

`9310e4e7b17126610e915624e8bc7b593391d844f6ab36beda2f020a429afd35`

**Modified SHA-256:**

`76bbb88975f68ec4e2e3ed35aec7350e25b468c26c441f549af3831ba4ea8ba6`

The difference between the original and modified hashes provides evidence that the file's contents changed.

## 6. Security Analysis

Wazuh successfully identified the modification and generated Rule 550, indicating an integrity checksum change.

The alert was associated with MITRE ATT&CK technique T1565.001 — Stored Data Manipulation.

In an operational environment, this type of detection could help identify unauthorised modifications to configuration files, application data or other important system resources.

However, a file integrity alert does not automatically indicate malicious activity.

A SOC analyst would need to determine whether the change was authorised, identify the responsible user or process where evidence permits, and assess the potential impact.

In this case, the modification was intentionally performed as part of an authorised security testing exercise.

### Detection Limitations

The Wazuh macOS agent used scheduled scanning rather than real-time monitoring.

Consequently, there may be a delay between a file modification and the generation of an alert.

File Integrity Monitoring also does not independently establish the identity or intentions of the person responsible for a modification.

## 7. Incident Classification

| Category | Assessment |
|---|---|
| Detection | Successful |
| Activity | Authorised test |
| Malicious Behaviour | Not observed |
| Security Impact | None identified |
| False Positive | No — the expected modification was correctly detected |
| Final Status | Closed — Test successful |

## 8. Conclusion

This project successfully demonstrated File Integrity Monitoring using Wazuh Cloud and a macOS endpoint.

After establishing an integrity baseline, Wazuh detected a modification to the monitored configuration file and generated a Level 7 alert.

The investigation confirmed that the file size, modification timestamp and cryptographic hashes had changed.

This exercise demonstrated the practical application of endpoint monitoring, SIEM alert analysis and detection validation.

### Wazuh FIM Detection

