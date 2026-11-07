# Incident Command, Coordination & Communication

Technically sound response still fails without command and communication. Someone must run
the incident, people must know their roles, and the right messages must reach the right
audiences — internal, legal, regulatory, and sometimes public — at the right time.

## Incident command
- **One incident commander (IC):** owns coordination and decisions (not necessarily the most
  technical person — the best coordinator). Keeps the response organized, tracks status, makes/
  escalates calls, and protects the responders from distraction. For long incidents, rotate to
  avoid burnout.
- **Roles:** lead investigator/forensics, containment/remediation lead(s), a **scribe** (maintains
  the incident log), a **communications** lead, and **liaisons** to legal, leadership, HR, and
  external parties. On a small team one person wears several hats — but name them explicitly.
- **Incident log / single source of truth:** timestamped record of every action, decision,
  finding, and communication. Essential for coordination, the post-incident review, and legal/
  regulatory defensibility. Start it at declaration.
- **War room** (physical/virtual) and a clear operational tempo (regular sync points), so effort
  is coordinated, not duplicated or dropped.

## Out-of-band communication
- Assume primary comms (corporate email, chat, VoIP) may be **compromised or monitored** by the
  attacker. Use a separate, trusted channel for incident coordination so you don't broadcast your
  response to the adversary. Pre-establish it in preparation.

## Internal communication
- **Leadership/executives:** concise, factual, regular updates — impact, status, decisions needed,
  and next update time. Avoid speculation; give confidence levels. They own business-risk decisions
  (e.g. take a system down, pay a ransom).
- **Staff/affected teams:** need-to-know; clear instructions (e.g. "don't touch system X,"
  "reset your password"). Avoid rumor; one authoritative voice.
- Keep it factual and measured while facts are emerging — early theories are often wrong and can
  leak or mislead.

## Legal, regulatory & external
- **Legal counsel early:** may direct the investigation under **privilege**, advises on obligations
  and liability, and gates external statements. Loop in from the first hours.
- **Regulatory/breach notification:** many regimes impose strict timelines once a breach is known
  — e.g. **GDPR 72 hours**, various US state and sector laws (HIPAA, etc.), and contractual
  customer-notification clauses. Know them in advance; coordinate with `grc-analyst`. The forensic
  finding of **what data was affected and whose** drives who must be told. Missing a deadline is its
  own serious problem.
- **Insurer:** notify the cyber-insurance carrier early — policies often require it and provide
  (or mandate) specific IR/forensics/legal resources.
- **Law enforcement:** a leadership/legal decision; can help but has its own process. Decide
  deliberately, not reflexively.
- **Customers/partners/public & PR:** external statements go through comms + legal, coordinated and
  consistent. Say what's known and what's being done; don't over-promise or minimize. Prepare
  holding statements in advance for likely scenarios. Analysts supply facts; they don't issue public
  statements.

## Documentation & handoff
- Maintain the running log throughout; produce a final **incident report** (technical detail +
  executive summary) and preserve the evidence package. Capture actions for the post-incident
  review. Clean, documented handoffs across shifts (status, open threads, next steps).

## Common pitfalls
- No incident commander → chaos, duplicated/dropped work, no decisions.
- No/weak incident log → can't reconstruct, can't defend findings, weak lessons-learned.
- Coordinating over compromised channels → tipping off the attacker.
- Engaging legal/insurer too late; missing a notification deadline.
- Ad-hoc or premature external statements; speculation presented as fact.
- Responder burnout on long incidents with no rotation.
