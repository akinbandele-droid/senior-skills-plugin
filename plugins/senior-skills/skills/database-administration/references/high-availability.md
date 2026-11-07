# High Availability, Replication & Disaster Recovery

Keep the database serving through failures — node crashes, disk loss, zone/region outages —
and be able to recover from a disaster. HA reduces downtime; backups (separate concern)
protect against data loss and corruption. You need both; HA is not a backup.

## Replication
Maintaining copies of the data on other nodes.
- **Synchronous:** a commit waits until a replica confirms — **zero data loss** (RPO≈0) on
  failover, at the cost of write latency and sensitivity to replica/network health. Use for the
  lowest RPO.
- **Asynchronous:** primary commits without waiting — low latency and good for read scaling and
  geo-distance, but a failover can **lose the last unreplicated transactions** (RPO > 0). Know and
  accept the possible loss.
- **Replication lag:** asynchronous replicas trail the primary. Reads from a lagging replica are
  **stale** — fine for reporting, dangerous for read-after-write expectations. Monitor lag; route
  reads accordingly.
- Topologies: primary→replica (read scaling, failover), multi-replica, cascading, and **multi-
  primary** (write anywhere — powerful but introduces conflict resolution and consistency
  complexity; use only when truly needed).

## Failover
- **Automatic** (managed/cluster manager promotes a replica on failure — faster RTO, but needs
  fencing to avoid split-brain) vs. **manual** (controlled, safer against false triggers, slower).
- **Split-brain:** two nodes both think they're primary → divergent/corrupt data. Prevent with
  quorum/witness, fencing/STONITH, and proper cluster config. A real and serious risk.
- After failover, clients must reconnect to the new primary — use a virtual IP, DNS, proxy, or a
  driver that's failover-aware. Test the whole path.
- **Test failover regularly** (game days). An untested HA setup often fails when it matters —
  wrong RTO, stale replica promoted, clients not reconnecting, or split-brain.

## Clustering & shared storage
- Shared-disk clusters (e.g. Oracle RAC) vs. shared-nothing replication. Cloud managed databases
  provide much of this (multi-AZ, automated failover, read replicas) — understand what the managed
  tier actually guarantees (RPO/RTO, sync vs. async) rather than assuming.

## Disaster recovery (DR)
- HA usually covers a node/zone; **DR** covers losing a whole region/site. Keep a cross-region
  replica and/or off-site backups. Define DR RPO/RTO (often looser than local HA) and a **tested**
  DR runbook including failback.
- Spread across **failure domains** (racks, availability zones, regions) so one domain's loss
  doesn't take all copies.

## Consistency tradeoffs
- Distributed databases trade among consistency, availability, and partition tolerance (CAP) and
  latency (PACELC). Pick the model the application needs; understand what anomalies async/eventual
  consistency can expose (stale reads, lost updates) and design around them.

## Common pitfalls
- Treating replication as a backup (it faithfully replicates a `DROP TABLE` or corruption too).
- Async replication assumed lossless → surprise data loss on failover.
- Reading from lagging replicas where fresh data is required.
- Untested failover; split-brain from missing quorum/fencing; clients not reconnecting.
- Only local HA, no cross-region DR; DR runbook never rehearsed (and no failback plan).
