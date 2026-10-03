# Investigation Summary: Sysmon "Suspicious Process" Alert (svchost.exe)

**Environment:** Home SOC lab (Lab-Win11 agent → Wazuh manager on Lab-SIEM01)
**Date of alert:** 2026-09-03

## Alert Details
- **Wazuh Rule:** 61618 — "Sysmon - Suspicious Process - svchost.exe"
- **Rule Level:** 12
- **MITRE ATT&CK:** T1055 (Process Injection) — Tactics: Defense Evasion, Privilege Escalation
- **Source:** Sysmon Event ID 1 (Process Create), via `Microsoft-Windows-Sysmon/Operational` channel
- **Host:** Lab-Win11.corp.local
- **Process:** `C:\Windows\System32\svchost.exe -k LocalSystemNetworkRestricted -p`
- **Fired times:** 17 (cumulative count for this rule ID, not a burst count)

## Initial Triage
Reviewed the raw Sysmon event and Wazuh's rule metadata side by side. Confirmed the rule fired due to a heuristic associated with process injection/masquerading characteristics rather than a known-bad hash or signature. Noted the process ran under `NT AUTHORITY\SYSTEM` with System integrity — expected for a legitimate `svchost.exe` service host process, so severity/context needed further validation before treating this as confirmed malicious.

Also observed several other Process Create events for unrelated services in the same short timeframe, which became a key thread for the follow-up investigation below.

## Follow-Up Investigation
1. **Parent-process anomaly identified:** The event showed a real `ParentProcessId` (808) but a fully blank parent record — no `ParentImage`, `ParentCommandLine`, `ParentUser`, or valid `ParentProcessGuid`. Normally `svchost.exe` should show clear lineage from `services.exe`. This orphaned-parent pattern is the specific signature the rule keyed on.
2. **Timing correlation:** Confirmed the alert timestamp fell immediately after an OS reboot (~6:19 PM) on Lab-Win11. Sysmon cannot retroactively reconstruct process lineage for processes that existed before it starts monitoring — a well-documented source of false-positive "orphaned parent" alerts right after a reboot or Sysmon service restart.
3. **Breadth check:** Verified multiple other Process Create events for different, unrelated services showed the identical blank-parent pattern within the same narrow window. A real, targeted injection attempt would not be expected to produce this same fingerprint simultaneously across many unrelated legitimate processes — this pattern is much more consistent with a systemic startup/telemetry gap than an attack.
4. **Static file verification:** Confirmed the digital signature on the on-disk `svchost.exe` two ways — via Windows Explorer (Properties → Digital Signatures → "This digital signature is OK") and independently via Microsoft's Sigcheck utility ("Signed"). This rules out a disguised/substituted binary.

## Conclusion
**Determination: False Positive.**
The alert was triggered by a Sysmon telemetry gap following an OS reboot, not genuine process injection. Evidence combined three independent angles — timing (reboot-aligned), breadth (identical pattern across unrelated processes), and static verification (valid digital signature, confirmed two ways) — rather than relying on a single data point.

**Scope note:** Signature verification confirms the on-disk binary is authentic and unmodified; it does not, by itself, prove the *running process's memory* was never tampered with post-launch. In this case that distinction doesn't change the conclusion, since the underlying detection concerned the process-creation event's parent lineage rather than any later runtime behavior — but it's a distinction worth stating precisely rather than treating "valid signature" as a blanket clean bill of health.

## Recommendation (Detection Tuning)
Since this orphaned-parent pattern is a recurring, benign artifact tied to Sysmon startup after a reboot, a mature next step would be to tune Rule 61618 — e.g., suppress or downgrade its severity when it fires within a short window (~2 minutes) of a Sysmon service start event. This reduces alert fatigue from a known-benign pattern without losing the rule's detection value the rest of the time.
