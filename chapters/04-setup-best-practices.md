
# Chapter 4: Setting Up Kafka the Right Way

Deploying Kafka correctly is crucial to avoid issues down the road. This chapter covers deployment options, common pitfalls, and configuration tips.

## Deployment Approaches

- **Bare metal or VMs**: Gives full control over resources but requires managing OS, networking, and scaling.
- **Docker containers**: Useful for development and testing; can be used in production with proper orchestration.
- **Kubernetes**: Offers container orchestration, auto-scaling, and self-healing, but adds operational complexity.

Choose the approach that fits your team’s expertise and infrastructure.

## Common Mistakes in Setup

- Underestimating disk throughput and IO capacity.
- Misconfiguring retention policies leading to disk full errors.
- Using too few partitions for the workload, limiting scalability.
- Skipping durability settings like replication and min.insync.replicas.
- Running Zookeeper and Kafka on the same hosts without resource isolation.

## Configuring Durability and Retention

- Set **replication.factor** to at least 3 for production clusters.
- Configure **min.insync.replicas** to ensure producers can guarantee message durability.
- Use **log.retention.hours** or **log.retention.bytes** to manage disk usage.
- Tune **log.segment.bytes** and **log.segment.ms** for efficient log rolling.

## Initial Cluster Testing & Benchmarking

- Use tools like `kafka-producer-perf-test.sh` and `kafka-consumer-perf-test.sh` to validate throughput.
- Test failover scenarios by killing brokers and observing behavior.
- Monitor key metrics like request latency, ISR counts, and under-replicated partitions.

---

Proper setup avoids many headaches and lays the foundation for stable Kafka operation.

---

Shall I continue with Chapter 5?
