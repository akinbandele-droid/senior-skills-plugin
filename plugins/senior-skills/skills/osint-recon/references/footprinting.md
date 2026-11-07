# Footprinting & Asset Discovery (Passive)

Build the target's external footprint from open sources — without sending traffic to its
infrastructure. Authorization and scope first (see SKILL.md section 0).

## Organization footprint
- **Identity:** legal names, trading names, brands, subsidiaries, and **acquisitions**
  (newly-acquired companies drag in forgotten assets). Business registries and public filings.
- **People/tech hints (org-level, privacy-respecting):** job postings often reveal the tech
  stack, tools, and cloud providers; public engineering blogs, conference talks, and public
  code repositories reveal architecture and systems. Keep the focus on the org's exposure,
  not individuals' personal lives.

## Domain & DNS
- **Registered domains:** WHOIS/registration data and reverse-WHOIS to find other domains the
  org registered. Watch for typo/brand-protection domains and old ones still live.
- **DNS records:** A/AAAA, CNAME, MX, NS, TXT (SPF/DKIM/DMARC reveal mail providers and
  sometimes third-party services), SOA. All retrievable from public resolvers.
- **Subdomain discovery (passive):**
  - **Certificate Transparency logs** — issued TLS certs list hostnames; a rich, passive source.
  - **Passive DNS** datasets and public asset/search databases that already indexed the org.
  - Search engines and public internet-scan datasets (e.g. indexed services) — reading their
    results is passive; running your own scan is active (defer to `pentest`).
  - Dedupe and keep provenance; resolving/verifying hosts yourself crosses into active.

## Infrastructure & IP space
- **IP ranges / ASNs:** registry data (RIR/ASN lookups) to find netblocks the org owns; map
  domains/hosts to their hosting (own DC vs. cloud/CDN — affects who owns the risk and scope).
- **Cloud & third-party assets:** storage buckets/blobs, SaaS tenants, code/package registries,
  status pages, and other services tied to the org — note them and **respect scope** (third-party
  services are often explicitly out of scope).

## Technology fingerprinting (passive)
- From already-indexed data and public sources: web stack, frameworks, CMS, analytics, CDNs,
  email and SaaS providers. Reading public indexes is passive; probing the live site to
  fingerprint it is active enumeration (that belongs in `web-pentest`).

## Hygiene
- Keep a structured inventory: asset → type → source → first/last seen → confidence → in/out
  of scope. Note dates (this is a point-in-time snapshot). Flag surprising or forgotten assets
  (shadow IT) early — they're frequent high-risk findings.

## The passive/active line (important)
Passive = consuming data that already exists in public/third-party sources. **Active** =
sending any traffic to the target (DNS brute-force against its servers, port/vuln scanning,
hitting its web apps, resolving to check liveness). Active enumeration is the start of the
`pentest` workflow and requires its authorization and scope — don't drift across the line
under the banner of "recon."
