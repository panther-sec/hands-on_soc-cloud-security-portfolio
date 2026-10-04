# Cybersecurity Incident Report

| Incident ID | LAB-20260914-01 |
|---|---|
| Incident Title | SOC141 - Phishing URL Detected |
| Date/Time Discovered | 2021-03-22 13:23:54 |
| Severity Level | Low |
| Incident Handler | Me |
| Status | Closed |

# 1. Executive Summary

Provide a high-level overview of the incident, impact, and current resolution status for leadership.

A phishing email reached user Ellie's inbox and the embedded link was clicked. Threat intelligence (Hybrid Analysis) confirmed the URL as tied to a known malicious domain; no evidence of data exfiltration was found. The URL was added to the block list and the affected workstation was contained. Incident closed, assessed as Low severity due to single-user impact and no confirmed compromise.

# 2. Timeline of Events

Chronological log of key events from initial entry to closure.

| Date / Time (UTC) | Event / Action Taken | Details / Notes |
|---|---|---|
| 2021-03-22 13:23:54 | Email Delivery | Phishing email bypassed the email gateway and landed in the user's inbox. |
| [HH:MM - not provided in alert] | First User Report | User reported the suspicious email via the mail client's built-in “Report Phishing” button. |
| [HH:MM - not provided in alert] | SOC Triage | Incident confirmed by SOC; malicious indicators (URL, sender IP) extracted for containment. |
| 2026-09-14T18:49:02.735164+00:00 | Containment Initiated | Sender blocked and malicious email purge initiated across the tenant. |

# 3. Indicators of Compromise (IOCs)

Technical artifacts used to identify, track, and block the threat.

| Indicator Type | Value |
|---|---|
| Sender Email Address | Not provided in alert |
| Sender IP Address | 91.189.114.8 |
| Subject Line | Not provided in alert |
| Malicious Link / URL | http://mogagrocol.ru/wp-content/plugins/akismet/fv/index.php?email=ellie@letsdefend... |
| Attachment File Name / Hash | N/A |

**Note: ***the malicious URL's query string embeds the victim's own email address (“?email=ellie@...”), a strong indicator this was a personalized/targeted phishing link — commonly used on credential-harvesting pages that pre-fill the victim's email to appear more legitimate — rather than a generic mass-distributed spam link.*

# 4. Investigation & Analysis

Technical details regarding the attack vector, scope, and payload execution.

- **Email Gateway Evaluation: **email management tool flagged url as phishing URL

- **Scope Assessment: **Only one user (ellie) clicked URL which was allowed by device

- **Host/Identity Impact: **No information in alert provided on what if any actions were taken once user clicked. Recommend reviewing EDR/endpoint telemetry on EmilyComp for any process execution or outbound network connections following the click, to confirm whether this was a benign landing page or a credential-harvesting/payload-delivery page.

# 5. Remediation & Prevention

Actions taken to mitigate the current threat and strategic changes to prevent a recurrence.

- **Immediate Containment Actions: ***Blocked* URL and quarantined workstation (EmilyComp)

- **Eradication Verification: ***Confirmed 0 remaining instances of the email in the tenant; blocklist propagation confirmed across firewalls.]*

- **Strategic Prevention / Lessons Learned: **Recommend targeted phishing-awareness training emphasizing URL inspection before clicking, and consider DNS-layer filtering or stricter scrutiny of uncommon top-level domains (e.g., .ru) frequently associated with malicious campaigns.
