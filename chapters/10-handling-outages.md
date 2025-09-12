
# Chapter 10: Handling Outages & Recovering

Even the best Kafka deployments face outages. Knowing how to respond quickly minimizes downtime and data loss.

## When Brokers Die

- Detect broker failures using monitoring alerts.
- Kafka automatically triggers leader elections for affected partitions.
- Be aware of downtime during leader elections and potential increased load on surviving brokers.

## Zombie Leaders and ISR Issues

- Sometimes a broker thought dead returns unexpectedly, causing "zombie leader" conflicts.
- Monitor ISR (in-sync replica) shrinkage to avoid data loss risks.
- Use controlled broker shutdowns and rolling restarts to maintain cluster stability.

## Restoring from Backups

- Kafka’s immutable logs reduce the need for backups, but critical data may require snapshots.
- Use tools like MirrorMaker or export connectors for replication and disaster recovery.
- Test recovery procedures regularly.

## Avoiding Data Loss

- Set appropriate replication and min.insync.replicas.
- Ensure producers use acknowledgments and idempotence.
- Monitor and resolve under-replicated partitions promptly.

---

Preparedness and swift action are key to surviving outages without losing data.

---

Would you like me to write Chapter 11 now?
