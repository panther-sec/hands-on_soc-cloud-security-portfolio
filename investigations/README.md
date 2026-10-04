# Investigation Index

A running index of alert/incident investigations from the home lab. Each entry links to a standalone write-up following the same format: **Alert Details → Initial Triage → Follow-Up Investigation → Conclusion → Recommendation**.

| # | Date | Title | Source | MITRE Technique | Tactic(s) | Outcome | Write-up |
|---|------|-------|--------|------------------|-----------|---------|----------|
| 01 | 2026-09-03 | Sysmon "Suspicious Process" Alert (svchost.exe) | Wazuh (Sysmon) | T1055 – Process Injection | Defense Evasion, Privilege Escalation | False Positive — post-reboot Sysmon telemetry gap | [01-svchost-process-injection-fp](01-svchost-process-injection-fp/writeup.md) |
| 02 | 2026-09-04 | "Trojaned Version of File Detected" — /usr/bin/md5sum | Wazuh (rootcheck) | N/A — signature-based host integrity check | N/A | False Positive — Ubuntu 26.04 Rust-coreutils signature-freshness gap (matched vendor issue) | [02-coreutils-trojaned-file-fp](02-coreutils-trojaned-file-fp/writeup.md) |
| 03 | 2026-09-14 | SOC141 — Phishing URL Detected | LetsDefend | T1566 – Phishing | Initial Access | Confirmed malicious — blocked, contained, no data exfiltration | [03-phishing-url-detected](03-phishing-url-detected/writeup.md) |

## How to add a new entry

1. Copy [`templates/investigation-writeup-template.md`](../templates/investigation-writeup-template.md) as the starting point for a new write-up.
2. Create a new folder: `NN-short-descriptive-name/`, incrementing the number, and save the write-up as `writeup.md` inside it.
3. Add one row to the table above — date, short title, source tool, MITRE technique/tactic, outcome (True Positive / False Positive / Benign / Escalated), and a link to the write-up.
4. Keep each write-up self-contained and skimmable — this index is the menu; the individual files are the detail you pull up when someone wants to go deeper.

## Portfolio pieces (per 12-week training plan)

- **Week 6 deliverable** (phishing/SOC alert): entry 03 above.
- **Week 11 deliverable** (cloud investigation via CloudTrail → Splunk/Wazuh pipeline): to be added once completed.
