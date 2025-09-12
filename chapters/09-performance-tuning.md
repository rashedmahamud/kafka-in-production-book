
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
## Case study
- How Parallel Consumption works
  -A Kafka topic is divided into multiple partitions. The consumer group protocol ensures that each partition is assigned to only one consumer within the same consumer group. Therefore, to achieve parallelism, you can't have a single consumer processing multiple messages in parallel from the same partition. Instead, you create multiple consumer instances within the same consumer group.

## Here are the key steps:

-Increase Partitions: First, ensure your Kafka topic has more partitions than the number of consumers you intend to run in parallel. A common practice is to have a number of partitions equal to the desired concurrency level. If the number of consumers exceeds the number of partitions, some consumers will be idle.

-Run Multiple Consumers: Launch multiple instances of your consumer application, all configured with the same group.id.

-Automatic Partition Assignment: When these consumers start, they will join the consumer group. The Kafka broker and the consumer group protocol will automatically handle the assignment of partitions among the available consumers. For example, if you have a topic with 4 partitions and you start 4 consumer instances, each instance will be assigned one partition.

-Independent Processing: Each consumer instance will then independently and concurrently consume messages from its assigned partition(s). Since each consumer is running on a different thread or even a different machine, they can process messages from their respective partitions in parallel.

Implementation
## There are two main ways to implement this in code:

- Multiple Threads: In a single application, you can create and run multiple instances of your BackgroundService class on separate threads. Each BackgroundService instance would be a unique Kafka consumer instance with the same group.id.

- Multiple Processes: The more common and robust approach is to deploy your application as multiple, independent processes (e.g., in a Docker container or on separate virtual machines). Each process runs one consumer instance. This provides better fault tolerance and resource isolation.

- You do not use Task.Run on a single consumer.Consume call to achieve true parallelism across partitions. The Task.Run pattern is for achieving concurrency by offloading blocking calls, not for parallel message consumption across different partitions. To consume in parallel, you must have separate, independent consumer instances reading from different partitions.
