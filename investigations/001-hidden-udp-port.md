Investigation 001 — Hidden UDP Port

Platform: Wazuh Cloud
Endpoint: macOS (Apple Silicon)
Detection: Rootcheck — Rule 510
Severity: Level 7
Status: Inconclusive — monitoring required

1. Incident Summary

Wazuh generated an alert indicating that UDP port 59797 was potentially hidden from normal system enumeration. The alert suggested a possible rootkit-related anomaly, requiring further investigation.

2. Investigation

I performed the following checks using macOS Terminal:

Check 1 — Identify the process using the port

sudo lsof -nP -iUDP:59797

Result: No matching processes were returned.

Check 2 — Check the operating system's UDP port table

netstat -anv -p udp | grep 59797

Result: No matching entries were returned.

Check 3 — Inspect currently visible UDP sockets

sudo lsof -nP -iUDP

Result: Multiple system services and applications were visible, but no socket associated with UDP port 59797 was identified.

3. Analysis

The initial Wazuh detection could not be independently reproduced during the manual investigation.

One possible explanation is that a temporary network socket closed before the investigation began. Another possibility is a discrepancy between Wazuh's detection method and macOS network reporting.

The available evidence does not establish that malware is present, but it also does not conclusively demonstrate a false positive.

4. Conclusion

The investigation found no current evidence of a process using UDP port 59797.

Disposition: Unresolved — no current corroborating evidence.

Further monitoring and analysis of recurring alerts are recommended before closing the investigation.
