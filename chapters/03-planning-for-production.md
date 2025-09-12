
# Chapter 3: Planning for Production

Getting Kafka ready for production means making decisions early that will impact reliability, scalability, and maintenance.

## Self-Hosted vs Managed Kafka

- **Self-hosted Kafka** gives you full control but requires operational expertise to manage brokers, storage, and upgrades.
- **Managed Kafka services** like Confluent Cloud or AWS MSK reduce operational overhead but may come with limitations or cost tradeoffs.
- Evaluate your team’s skills, budget, and SLAs when choosing.

## Hardware and SaaS Considerations

- Kafka’s performance depends heavily on underlying infrastructure:
  - Use fast disks (NVMe/SSD preferred) with enough throughput.
  - Ensure network capacity supports your peak message rate.
- For self-hosted Kafka:
  - Plan for dedicated brokers; avoid co-locating resource-intensive workloads.
- For SaaS:
  - Understand scaling limits, SLAs, and integration options.

## High-Availability Architecture

- Use replication factors of at least 3 for critical data.
- Deploy brokers across multiple availability zones or data centers.
- Consider geo-replication (MirrorMaker) for disaster recovery or multi-region setups.

## Choosing Partition Counts and Replication Factors

- More partitions improve parallelism but increase broker load and complexity.
- Partition count is hard to change after deployment, so estimate growth carefully.
- Replication factor balances durability vs resource usage; 3 is a common baseline.

## Summary

Good planning lays the foundation for a robust Kafka deployment. You’ll save time and headaches by making thoughtful decisions upfront.

---

Next up: Setting up Kafka the right way — how to deploy and configure clusters for production use.

