# Wazuh RDP Brute Force Detection Lab

A home SOC lab built to simulate, detect, and document a simulated attack
using Wazuh SIEM — from attack execution to custom detection rule
engineering.

## Lab Architecture
- **Wazuh Manager** — Ubuntu Server (SIEM / log collection & alerting)
- **Attacker** — Kali Linux
- **Target** — Windows 11 (monitored endpoint, Wazuh agent installed)

All machines run as VMware virtual machines on an isolated internal network.

## What This Lab Demonstrates
1. Deploying and configuring a Wazuh SIEM with agent-based monitoring
2. Simulating a real RDP brute force attack using Hydra
3. Analyzing raw Windows Security logs (Event ID 4625) in Wazuh
4. Identifying a detection gap in the default ruleset
5. Writing, debugging, and validating a custom Wazuh detection rule
6. Mapping the detection to MITRE ATT&CK (T1110 - Brute Force)
7. Documenting the incident in a professional SOC-style report

## Project Structure

```
wazuh-detection-lab/
├── screenshots/   # Evidence of each detection stage
├── reports/       # Incident reports
└── rules/         # Custom Wazuh detection rules
```

## Reports
- [Incident Report 01 — RDP Brute Force Attack](reports/01-rdp-bruteforce.md)

## Notes
This project is actively being expanded with additional attack scenarios
(e.g. port scanning, suspicious process execution) to broaden detection
coverage.
