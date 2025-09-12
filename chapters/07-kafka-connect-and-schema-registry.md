
# Chapter 7: Kafka Connect & Schema Registry

Integrating Kafka with other systems and managing data schemas are essential parts of production pipelines.

## Using Kafka Connect in Production

- Kafka Connect provides a scalable way to ingest and export data.
- Choose the right connectors for your sources/sinks, and tune tasks for parallelism.
- Monitor connector health and task status regularly.
- Plan for connector failures and retries to avoid data loss.

## Schema Evolution and Compatibility

- Use **Schema Registry** to manage schemas centrally.
- Enforce schema compatibility rules (backward, forward, full) to avoid breaking consumers.
- Version your schemas and use Avro/Protobuf/JSON Schema formats for data consistency.

## Avoiding Downstream Data Breakages

- Always test schema changes in staging before production.
- Use backward-compatible schema changes (adding optional fields) for smooth upgrades.
- Be mindful of consumer schema versions and how they handle missing or extra fields.

---

Proper schema management and connector usage prevent many integration headaches.

---

Shall we continue to Chapter 8?
