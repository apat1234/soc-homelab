
## Phase 1 — SIEM Deployment

### Objective
Deploy a cloud-based SIEM and monitor security
events from a macOS endpoint.

### Environment
- Wazuh Cloud, Sydney region
- Wazuh macOS Agent
- Apple Silicon MacBook (8 GB RAM)

### Initial Results
- Endpoint agent successfully connected
- 76 alerts observed in the initial dashboard
- 0 alerts at severity level 12 or above
- Alert groups included SCA, rootcheck, sudo,
  syslog, and OSSEC

### Next Steps
- Investigate individual security alerts
- Configure File Integrity Monitoring
- Generate controlled test events
- Document detection and response findings
