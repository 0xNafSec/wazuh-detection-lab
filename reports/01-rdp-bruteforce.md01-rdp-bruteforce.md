# Incident Report 01 — RDP Brute Force Attack

## Summary
A brute force attack against RDP (port 3389) on host `windows-victim` 
(192.168.163.130) was simulated from an attacker machine `kali` 
(192.168.163.128) using Hydra. The attack was detected via Windows 
Security Event ID 4625 and a custom Wazuh detection rule.

## Timeline
- Target hardened: RDP enabled, firewall opened, user added to 
  Remote Desktop Users group, logon auditing enabled (success/failure).
- Attack executed from Kali using Hydra against a 5-entry password list.
- Windows logged 5 failed logon attempts (Event ID 4625) from source 
  IP 192.168.163.128, logon type 3 (network).
- Default Wazuh rules logged the events but did not raise an alert.
- Custom rule (ID 100100) was written to trigger when 5+ matches of 
  rule 60122 (Windows logon failure) occur within 120 seconds.
- Rule fired successfully, confirmed in Wazuh alerts log and dashboard.

## Detection
- **Rule ID:** 100100
- **Level:** 10
- **Logic:** frequency=5, timeframe=120s, if_matched_sid=60122
- **MITRE ATT&CK:** T1110 (Brute Force)

## Evidence
- `screenshots/01-failed-logon-4625.png`
- `screenshots/02-bruteforce-from-kali-4625.png`
- `screenshots/03-custom-rule-triggered-100100.png`

## Lessons Learned
- Default Wazuh ruleset logs individual failed logons but does not 
  correlate repeated attempts into a single alert — custom rules are 
  needed to detect brute force patterns.
- `if_matched_sid` requires pairing with `frequency`/`timeframe`; using 
  `if_sid` with frequency options causes a rule load error.
- RDP brute forcing tools can behave inconsistently with parallel 
  connections; reducing threads (`-t 1`) and adding delay (`-W 3`) 
  improved reliability.

## Recommended Response
- Enable account lockout policy after N failed attempts.
- Restrict RDP access via VPN or jump host instead of direct exposure.
- Alert SOC on rule 100100 trigger for immediate investigation.
