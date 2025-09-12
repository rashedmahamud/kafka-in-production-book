
# Chapter 9: Performance Tuning at Scale

Scaling Kafka for high throughput and low latency requires careful tuning of brokers, JVM, and partitions.

## Broker Tuning

- Adjust **num.network.threads** and **num.io.threads** based on workload.
- Configure **log.flush.interval.messages** and **log.flush.interval.ms** for durability vs performance.
- Tune **replica.fetch.max.bytes** and **replica.fetch.wait.max.ms** to optimize replication.

## JVM Tuning

- Monitor garbage collection (GC) pauses and tune JVM heap size accordingly.
- Use G1GC or ZGC for better latency in newer Kafka versions.
- Avoid long GC pauses which cause broker unavailability.

## Partition Strategies

- Balance partition count to achieve parallelism without overwhelming brokers.
- Consider sticky partitioning or key-based partitioners to improve cache locality.
- Use partition reassignment carefully during scaling.

## Cluster Expansion Tips

- Add brokers gradually and monitor cluster health.
- Rebalance partitions to evenly distribute load.
- Plan for downtime or rolling upgrades.

---

Proper tuning avoids bottlenecks and keeps Kafka responsive at scale.

---

Shall I prepare Chapter 10 next?
