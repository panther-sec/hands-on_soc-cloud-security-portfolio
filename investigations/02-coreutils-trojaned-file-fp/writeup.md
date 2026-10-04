# Investigation Summary: "Trojaned Version of File Detected" — /usr/bin/md5sum

**Environment:** Home SOC lab (Lab-SIEM01 — Ubuntu 26.04 LTS, self-monitored via rootcheck)
**Date of alert:** 2026-09-04

## Alert Details
- **Wazuh Rule:** 510 — "Host-based anomaly detection event (rootcheck)"
- **Rule Level:** 7
- **Detection type:** rootcheck generic trojan signature match (not Sysmon/EDR — a periodic host integrity scan)
- **Flagged file:** `/usr/bin/md5sum`
- **Signature matched:** Generic (`bash|^/bin/sh|file\.h|proc\.h|/dev/[^cfhlnrstuw]|^/bin/.*sh`)
- **Fired times:** 20

## Initial Triage
Recognized rule 510 as a rootcheck signature-based check, distinct from the Sysmon/EDR-based alerts investigated previously — it flags binaries whose internal strings match known generic trojan patterns in Wazuh's bundled `rootkit_trojans.txt` signature file. A core system utility like `md5sum` being flagged warranted skepticism of a real compromise given how heavily used and easily monitored that binary is, but it still required actual verification rather than dismissal on instinct alone.

## Follow-Up Investigation

**1. External research — matched to known vendor issues.** Searched and found two relevant GitHub issues on the Wazuh repository:
- **wazuh/wazuh#32142** — same rule (510) false-positive on Debian 13, closed via an actual code fix (PR #35927).
- **wazuh/wazuh#35704** — an exact match: same rule (510), same binary (`/usr/bin/md5sum`), same OS (**Ubuntu 26.04 LTS**) as this lab environment. Root cause documented there: Ubuntu 26.04 ships a Rust-rewritten coreutils (`uutils`), and the new binaries' internal strings coincidentally match legacy generic trojan signatures written before the rewrite existed. Closed as a duplicate of #32142.

**2. Internal hash verification — first attempt was incomplete, caught and corrected.**
- `dpkg -S /usr/bin/md5sum` → owned by `coreutils-from-uutils`, a small transitional meta-package.
- `sudo dpkg --verify coreutils-from-uutils` → returned nothing (no output).
- `sudo debsums coreutils-from-uutils` → returned only two unrelated documentation files (`changelog.gz`, `copyright`) — **did not include `/usr/bin/md5sum` at all.** This revealed the first verification pass hadn't actually checked the binary in question; a clean/empty result had been mistaken for a confirmed-clean result.

**3. Traced the real target and re-verified.**
- `ls -la /usr/bin/md5sum` → revealed `/usr/bin/md5sum` is a **symlink**, not a standalone binary: `-> ../lib/cargo/bin/coreutils/md5sum`.
- `readlink -f /usr/bin/md5sum` → resolved to `/usr/lib/cargo/bin/coreutils/md5sum` (the `cargo` path segment independently corroborates the Rust-rewrite explanation from the GitHub issues).
- `dpkg -S /usr/lib/cargo/bin/coreutils/md5sum` → owned by the real package, `rust-coreutils`.
- `sudo debsums rust-coreutils | grep md5sum` → this time returned the actual binary itself: `/usr/lib/cargo/bin/coreutils/md5sum   OK`, confirming the real executable's hash matches what the package manager recorded at install time.

## Conclusion
**Determination: False Positive.**
The alert was caused by a signature-freshness gap in Wazuh's bundled generic trojan signatures, which pre-date Ubuntu 26.04's Rust-based coreutils rewrite and coincidentally match strings in the new binaries. This was independently confirmed two ways: (1) an exact-match vendor-acknowledged issue report describing the same rule, same binary, and same OS version, and (2) a verified-clean file-integrity check against the real underlying binary once the symlink and transitional meta-package were correctly traced.

**Process note worth keeping:** the first integrity-check attempt (`debsums coreutils-from-uutils`) returned a "clean-looking" result without actually covering the target file, due to `/usr/bin/md5sum` being a symlink to a binary owned by a different package. An empty/clean tool output was not automatically treated as confirmation — it was checked against what the tool had actually scanned before being accepted as evidence.

## Recommendation (Detection Tuning)
Added a local override rule suppressing this known signature-freshness false positive down to a low severity (rather than fully silencing it), scoped to the specific binaries confirmed affected in the vendor issue report:

```xml
<!-- Known FP: Ubuntu 26.04 Rust-coreutils binaries match legacy trojan signatures -->
<rule id="100102" level="3">
  <if_sid>510</if_sid>
  <field name="file">^/usr/bin/(md5sum|date|passwd|chfn|chsh)$|^/bin/(md5sum|date|passwd|chfn|chsh)$</field>
  <description>Rootcheck false positive on Ubuntu 26.04 coreutils (uucore) — matches known Wazuh issue, not an actual compromise. See wazuh/wazuh#32142, #35704.</description>
</rule>
```
