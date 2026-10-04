# Wazuh Rule Tuning Examples

Custom `local_rules.xml` overrides written during home-lab investigations, scoped narrowly to the specific false positives confirmed in [`investigations/01-svchost-process-injection-fp`](../../investigations/01-svchost-process-injection-fp/writeup.md) and [`investigations/02-coreutils-trojaned-file-fp`](../../investigations/02-coreutils-trojaned-file-fp/writeup.md).

Each rule downgrades severity rather than fully silencing the parent rule, preserving an audit trail while cutting down on alert fatigue from a confirmed-benign pattern. See `local_rules_examples.xml` for the rule definitions and the linked write-ups for the full investigation and reasoning behind each one.

**Note:** these overrides are intentionally narrow (specific binaries, specific field matches) rather than broad suppressions — a tuning rule should reduce noise from a *confirmed* benign pattern without blinding the parent rule to a genuinely different, malicious instance of the same technique.
