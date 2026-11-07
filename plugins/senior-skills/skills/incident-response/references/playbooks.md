# Incident Playbooks

Pre-built response outlines for common incident types speed and standardize response. Each
follows the lifecycle (identify → contain → eradicate → recover → review) with
type-specific actions. Adapt to your environment; preserve evidence throughout.

## Ransomware
- **Identify:** scope which systems are encrypted/affected and the variant; find patient zero and
  dwell time; determine if data was **exfiltrated** (double-extortion) — that changes it into a
  breach with notification duties.
- **Contain:** isolate affected and at-risk systems **fast** to stop spread (it moves laterally and
  fast); disable affected accounts; protect/verify **backups** immediately (attackers target them).
- **Eradicate/recover:** remove the malware and persistence, fix the entry vector, reset credentials;
  **restore from clean backups**. Treat **paying the ransom** as a last-resort business/legal decision
  (sanctions/OFAC risk, no guarantee of recovery) — leadership + counsel, never the default.
- **Engage** counsel, insurer, and likely external IR early; preserve evidence for notification/claims.

## Phishing / Business Email Compromise (BEC)
- **Identify:** which accounts received/clicked/entered credentials; was a mailbox accessed? Look for
  attacker **mailbox rules** (auto-forward/hide), new **OAuth app grants**, and MFA changes.
- **Contain:** reset the password **and revoke active sessions/tokens**; remove malicious inbox rules
  and OAuth grants; re-enforce MFA; block the phishing sender/domain and pull the email org-wide.
- **Eradicate/recover:** check for fraudulent wire/payment instructions (BEC's real goal) and alert
  finance; hunt the same campaign across other users; restore trust in affected accounts.
- Watch for lateral spread from a compromised internal account (internal phishing is trusted).

## Account / credential compromise
- **Identify:** scope the account's activity — logins (impossible travel, new devices/IPs), actions
  taken, data accessed, privilege changes, and anything it created/granted.
- **Contain:** disable/reset + **revoke sessions and tokens**; remove attacker-added MFA/keys/grants.
- **Eradicate/recover:** determine blast radius (what the account could reach), reset related creds,
  and address how it was compromised (phish, reuse, leak — pair with `osint-recon`). Assume lateral movement.

## Data breach / exfiltration
- **Identify (critical):** determine precisely **what data, whose, and how much** was accessed or
  exfiltrated — this drives legal notification. Find the exfil channel and timeline.
- **Contain:** cut the exfil path (block C2/destination, isolate), stop ongoing loss.
- **Legal/regulatory:** engage counsel immediately — **notification clocks** (e.g. GDPR 72h, US state
  and sector laws, contractual) start on discovery. Coordinate with `grc-analyst`. Comms via the plan.
- **Recover/review:** eradicate access, notify affected parties/regulators as required, offer remedies
  (e.g. credit monitoring) where appropriate.

## Malware outbreak (non-ransomware)
- Identify the malware and spread; isolate infected hosts; block C2; eradicate (reimage where unsure)
  and patch the vector; restore and monitor for reinfection. Detonate samples only in an isolated lab.

## Insider threat (malicious or negligent)
- Handle with extra care: involve **HR and legal from the start**, protect evidence and chain of
  custody (likely used in employment/legal action), and limit who knows. Scope the person's access and
  actions; contain by adjusting access discreetly to avoid tip-off/retaliation. Balance monitoring
  with privacy/legal constraints.

## DDoS / availability
- Identify the attack type/vector; engage upstream/ISP/CDN and DDoS protection; apply rate limiting/
  filtering/scrubbing; scale/absorb where possible; communicate status. Watch for DDoS as a
  **smokescreen** for a concurrent intrusion — don't drop your guard on the rest of the environment.

## Using playbooks well
- They're **starting points, not scripts** — judgment and the live investigation override the generic
  steps. Keep them current and test them in tabletops. Every playbook assumes: incident command is up,
  the incident log is running, and evidence is preserved before destructive action.

## Common pitfalls
- Password reset without session/token revocation (BEC, account compromise).
- Missing exfiltration in a ransomware case (under-reporting a notifiable breach).
- Ransomware: not protecting backups; paying as a reflex without legal review.
- Insider case without HR/legal and custody discipline.
- Tunnel vision during a DDoS while an intrusion proceeds behind it.
