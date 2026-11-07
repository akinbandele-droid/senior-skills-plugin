# Exposures, Leaks & Sensitive Data

Often the highest-value recon output: things the organization is exposing that it
shouldn't be. Find them, don't exploit them — report for remediation. Authorization and
scope first.

## Leaked credentials & secrets
- **Breach-exposure data:** check whether the org's accounts/emails appear in known public
  breach datasets (via reputable exposure-check services). This tells the org which accounts
  need resets/MFA — a defensive win.
- **Secrets in public code:** API keys, tokens, passwords, private keys, and connection
  strings committed by mistake to public repositories (and their **commit history** — a secret
  deleted in the latest commit often still lives in history). Also package manifests, CI config,
  and public gists/snippets.
- **Handling:** if you find live credentials/secrets, **do not use them**. Record the exposure
  (what, where, when), treat the value itself as sensitive (redact in notes/reports), and report
  it for immediate **rotation/revocation**. Using found credentials is intrusion, outside recon.

## Exposed documents & files (dorking)
- Use search-engine operators ("Google dorking") to surface content indexed but not meant to be
  public: specific file types (configs, backups, spreadsheets, PDFs), directory listings, login
  and admin pages, error pages leaking paths, and documents on misconfigured public storage.
- Reading what a search engine already indexed is passive. **Downloading from the target's own
  servers, or accessing something clearly not intended to be public, can cross legal/active
  lines** — confirm scope and the passive/active boundary before pulling anything down.

## Metadata
- Public documents (PDF/Office/images) carry metadata: author usernames, internal software and
  versions, file paths, and sometimes internal hostnames or geolocation. Useful for footprinting;
  also a finding in itself (the org is leaking internal info) to report.

## Exposed services & interfaces
- Public internet-scan datasets may already show the org's exposed services, open admin panels,
  databases, dashboards, and devices — readable passively. Note them as attack surface and risk;
  **connecting to or probing them is active testing** for the `pentest` family.

## Social & brand exposure
- Look (at org level) for impersonation/typosquat domains and brand-abuse, and for sensitive
  info the org or its public channels overshare. Keep it organizational and privacy-respecting;
  don't profile individuals beyond the engagement's need.

## Prioritize for the report
1. **Live leaked credentials/secrets** — rotate now.
2. **Exposed sensitive data/documents** — take down / lock down.
3. **Forgotten/unexpected exposed assets and services** — decommission or secure.
4. General footprint info leakage (metadata, verbose stack disclosure).

## Don'ts
- Don't use, test, or "just verify" leaked credentials.
- Don't download or access material clearly not meant to be public without scope confirmation.
- Don't compile personal dossiers on individuals; keep to the org's security exposure.
- Don't store secrets in the clear; redact them in deliverables.
