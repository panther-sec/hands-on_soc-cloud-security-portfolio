# Investigation Summary: <Alert Title>

**Environment:** <e.g., Home SOC lab — Lab-SIEM01 Ubuntu 26.04 LTS, Wazuh + Splunk>
**Date of alert:** <YYYY-MM-DD>

## Alert Details
- **Source/Rule:** <e.g., Wazuh Rule 61618, Splunk search, LetsDefend alert ID>
- **Rule/Severity Level:** <e.g., Level 12>
- **MITRE ATT&CK:** <Technique ID and name> — **Tactic(s):** <tactic(s)>
- **Host / User:** <affected host, user>
- **Summary of raw event:** <one or two lines describing what fired>

## Initial Triage
What did the alert claim, and what was the first-pass read before digging deeper? Note anything that stood out immediately (expected vs. unexpected context, known-benign patterns, severity vs. plausibility).

## Follow-Up Investigation
Numbered or bulleted steps taken to confirm or rule out the finding. Include:
- External research (vendor docs, GitHub issues, CVEs, threat intel)
- Internal verification (log correlation, hash/signature checks, timing analysis)
- Anything that corrected an earlier assumption — show the work, not just the conclusion

## Conclusion
**Determination:** <True Positive / False Positive / Benign / Escalated>

State the determination plainly, then explain the reasoning in 2-4 sentences, referencing back to the strongest evidence from the Follow-Up section.

## Recommendation (Detection Tuning / Next Steps)
What should change going forward — a tuned detection rule, a process note, an escalation, or nothing (if the control worked as intended). Include a code block for any rule/config changes.
