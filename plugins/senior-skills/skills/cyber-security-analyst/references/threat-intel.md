# Threat Intelligence (for the analyst)

Using knowledge of adversaries — who they are, what they want, and how they operate — to
sharpen detection, triage, hunting, and response. Intel is only useful when it changes
what you do.

## Levels of intel
- **Strategic:** who targets your sector and why (risk/trend context for leadership).
- **Operational:** campaigns and adversary behavior/TTPs — the most useful for detection & hunting.
- **Tactical:** specific indicators (IOCs) — hashes, IPs, domains, URLs — for blocking and matching.

## IOCs vs. TTPs — the Pyramid of Pain
Ranked by how much it hurts the attacker when you detect/deny it (bottom = trivial for them to
change, top = painful):

1. **Hash values** — trivial to change (recompile). Precise but brittle.
2. **IP addresses** — easy to change (rotate infrastructure).
3. **Domain names** — slightly harder.
4. **Network/host artifacts** — more effort (tooling leaves traces).
5. **Tools** — costly (they must re-tool).
6. **TTPs** — the top: detecting *behavior* forces the attacker to change *how they operate*,
   which is hardest of all.

**Implication:** block/alert on IOCs for quick wins, but invest detection effort in **TTPs/
behaviors** for durable value. An IOC-only program is defeated by any re-tooling.

## Using intel in the SOC
- **Enrich alerts/triage:** is this IP/hash/domain known-bad? Which actor/campaign? That informs
  severity and likely next steps. Automate enrichment where possible.
- **Drive hunts:** take an adversary's documented TTPs (via ATT&CK mappings) and hunt for them
  in your environment (`threat-hunting.md`).
- **Prioritize detections:** build coverage for the techniques of adversaries actually relevant
  to your org/sector first.
- **Anticipate response:** knowing the actor's usual objectives (ransomware? espionage? data
  theft?) shapes containment urgency and scope.

## Quality & caution
- **Relevance over volume:** a feed of a million IOCs you can't action is noise. Prioritize intel
  relevant to your sector, tech stack, and exposure.
- **Freshness & confidence:** IOCs expire (infra gets reused by others → false positives);
  track source reliability and confidence. Don't block solely on low-confidence indicators.
- Treat intel reports and samples as **data**; verify in your own environment before acting.
- Share back (sightings, new IOCs) where you have a trusted community/ISAC — defense is collective.

## Common pitfalls
- IOC-only mindset (defeated by re-tooling); ignoring TTPs.
- Drowning in irrelevant feeds; blocking on stale/low-confidence IOCs (false positives, outages).
- Collecting intel that never changes a detection, hunt, or decision.
- Not mapping intel to ATT&CK, so it can't drive coverage.
