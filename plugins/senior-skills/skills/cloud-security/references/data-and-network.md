# Cloud Data & Network Protection

## Data protection
- **Encrypt at rest by default** (storage, databases, disks, backups, snapshots) and
  **in transit** (TLS everywhere; disable weak protocols). Make default encryption
  account-wide so new resources are covered automatically.
- **Key management:** use the provider KMS; control who can use/manage keys (separate
  those duties); enable rotation. For sensitive/regulated data consider customer-managed
  keys and, where needed, your own key material. Guard key-deletion.
- **Classify and minimize:** know where sensitive/regulated data lives; don't copy it into
  logs, non-prod, or analytics without controls. Apply retention and deletion policies.
- **Backups:** encrypted, access-controlled, tested restores, and protected from deletion
  (immutability/versioning) so ransomware or error can't wipe them.

## Storage exposure (the classic breach)
- Default buckets/blobs/object stores and managed databases to **private**. Turn on the
  **account-level "block public access"** controls so an individual resource can't be
  made public by accident.
- Audit for public exposure continuously (CSPM). Treat any public data store as an
  explicit, reviewed, justified exception — not a default.
- Beware overly broad bucket/object ACLs and presigned-URL misuse; prefer private access
  via identity, not public links.

## Network
- **Segment:** VPC/VNet with public/private subnets; put data stores and internal services
  in private subnets with no public IP. Use private endpoints/service links to reach
  provider services without the public internet.
- **Default-deny** security groups/NSGs/firewall rules; allow only the specific ports/
  sources needed. No `0.0.0.0/0` to admin ports (SSH/RDP/databases) — use bastion/SSM/
  just-in-time access instead.
- **Edge protection:** WAF for public web apps, DDoS protection, TLS termination with
  strong config. Keep management/control planes off the public internet.
- **Egress matters too:** restrict outbound where feasible to limit exfiltration and C2.

## Common pitfalls
- Public storage buckets / databases with public IPs and open security groups.
- Unencrypted snapshots/backups, or backups deletable by the same compromised identity.
- Admin ports open to the internet; flat networks with no segmentation.
- KMS key policies so broad they negate the encryption's access control.
- Sensitive data sprayed into logs, non-prod, and analytics unprotected.
