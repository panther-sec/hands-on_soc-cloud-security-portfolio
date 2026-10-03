# SOC Analyst & Cloud Security — Home Lab Portfolio

Hands-on SOC analysis and cloud security work from a self-built home lab, done alongside a 12-week structured training plan. Background: USAF veteran, former AWS Cloud Support Engineer, CompTIA Security+/Network+/A+ and AWS Solutions Architect – Associate certified, transitioning into SOC analysis and cloud security.

## Home Lab

VirtualBox-based lab running Active Directory (domain controller + certificate services), a combined Splunk Enterprise + Wazuh SIEM, Windows 11 and Kali Linux endpoints, all generating real telemetry (Sysmon, Windows Event Logs, rootcheck/file-integrity events) that gets triaged like a real SOC queue.

## Start here

| | |
|---|---|
| 🔍 [**Investigations**](investigations/) | Alert triage write-ups — what fired, how it was investigated, and the conclusion (true/false positive) with supporting evidence |
| 🧪 [**Labs**](labs/) | Hands-on build/configuration walkthroughs (SAML SSO, SIEM rule tuning) written up as knowledge-base articles |
| 📋 [**Templates**](templates/) | Reusable incident report and investigation write-up templates used across this portfolio |
| 📘 [**Docs**](docs/) | The 12-week training plan driving this work |

## Featured write-ups

- [**SAML SSO Testing with a Python Dummy Service Provider**](labs/saml-sso-dummy-sp/) — manually crafting and encoding a SAML AuthnRequest, inspecting IdP metadata and signing certificates, and debugging real gotchas (timestamp validation, MFA policy behavior) against a live Okta tenant.
- [**Sysmon False Positive: svchost.exe Process Injection Alert**](investigations/01-svchost-process-injection-fp/writeup.md) — tracing a Level 12 alert back to a post-reboot Sysmon telemetry gap using timing correlation, breadth analysis, and signature verification.
- [**Rootcheck False Positive: Trojaned coreutils on Ubuntu 26.04**](investigations/02-coreutils-trojaned-file-fp/writeup.md) — tracing a flagged system binary through a symlink to the real owning package, cross-referenced against upstream vendor issue reports.

## Why this repo exists

Most of what's here is deliberately about **process**, not just outcomes — what was checked, what assumption turned out wrong, and why a finding was ultimately called a false positive or confirmed malicious. That's the part of SOC work that doesn't show up on a certificate.
