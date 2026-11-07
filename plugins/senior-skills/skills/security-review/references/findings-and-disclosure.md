# Findings & Responsible Disclosure

A finding is only useful if the reader can understand the risk, reproduce it, and
fix it. Rate consistently, document reproducibly, and disclose responsibly.

## Severity rating
Use **CVSS** (v3.1 or v4.0) and include the full vector string so the score is
reproducible, then map to a band. Where possible, express impact in the owner's
terms (data at risk, systems affected, compliance) alongside the number.

- **Critical** — severe, immediate (e.g. unauthenticated RCE, full data
  compromise). Report out of band the moment it's confirmed.
- **High** — significant impact, realistic exploitation.
- **Medium** — meaningful risk, usually needs conditions or chaining.
- **Low** — limited impact or hard to exploit.
- **Informational** — hardening / best-practice, no direct exploit.

Rate by real exploitability and impact under the stated attacker model — not by
how interesting the bug is.

## Finding template

```
### [ID] Title (Severity — CVSS score + vector)

Status: Confirmed | Theoretical (unverified)
Location: path/to/file.ext:line  (and commit/version)
Class: CWE-XXX / OWASP category

Summary:
  One or two sentences: the weakness and why it matters.

Impact:
  Concrete consequence under the attacker model (what they gain/break).

Reproduction:
  1. Exact steps / inputs / request to trigger it. Use a benign proof
     (canary), redact real secrets/PII, and no destructive actions.

Evidence:
  Code excerpt, request/response, or output showing the issue. Redacted.

Remediation:
  Specific fix (code/config/control) + any interim mitigation.

References:
  CWE, OWASP, CVE, vendor/advisory links.
```

## Good findings
- **Confirmed vs. theoretical** is explicit — never imply certainty you don't have.
- A concrete **failing scenario** (inputs → wrong/unsafe result), not a vague worry.
- **Actionable remediation**, not just "sanitize input."
- Least-invasive proof; no weaponized exploit in the report body for high-severity
  issues unless the owner needs it and the channel is secure.

## Report structure
1. Executive summary — posture, themes, counts by severity (plain language).
2. Scope, version/commit, method, and limitations (the "open" coverage statement).
3. Findings, ordered by severity.
4. Strategic recommendations — root causes and systemic fixes.
5. Appendices — logs, full evidence, tool output.

## Responsible disclosure (third-party / open-source code)
- **Report privately first** to the maintainer/vendor or their security contact
  (`security.txt`, security advisory, or a dedicated address). Don't open a public
  issue with exploit detail.
- **Give a reasonable timeline** to fix before publishing details (commonly ~90
  days, shorter for actively-exploited issues — coordinate).
- **Coordinate** the public advisory/CVE with the maintainer; credit fairly.
- **Minimize harm** while researching: only test what you're authorized to, don't
  access or exfiltrate real user data, and never extort ("bug bounty" ≠ demanding payment).
- If the issue is being actively exploited or the vendor is unresponsive, escalate
  through a CERT/coordination body rather than dropping a public 0-day.

## Handling evidence
- Store proof securely and access-controlled; redact secrets/PII.
- Delete sensitive evidence per the engagement's data-handling rules once reported.
