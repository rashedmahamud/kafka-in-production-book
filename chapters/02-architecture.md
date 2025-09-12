
# Chapter 2: Kafka Architecture Refresher (Real-World Framing)

Before diving deeper into production use, it’s important to revisit Kafka’s core architecture and how its components interact in real-world deployments.

## Topics, Partitions, Brokers, and Zookeeper/KRaft

- **Topics** are logical streams of data. They are split into multiple **partitions**, which allow Kafka to parallelize data storage and processing.
- Each **partition** is an ordered, immutable sequence of messages, and it lives on a single **broker** (Kafka server).
- **Brokers** form a Kafka cluster. They handle storing data and serving client requests.
- Traditionally, **Zookeeper** managed broker metadata and leader election. Newer Kafka versions use **KRaft** mode to remove Zookeeper, simplifying architecture.
  
## Kafka’s Design Strengths

Kafka’s distributed log design brings many advantages:

- High throughput for publishing and consuming messages
- Built-in replication for fault tolerance
- Consumer groups for scalable, fault-tolerant message processing
- Durable storage with configurable retention policies

## What Bites You in Production?

While Kafka’s design is elegant, running it in production presents challenges:

- Partition leader elections can cause short downtime or performance dips
- Consumer rebalances may lead to message processing delays
- Misconfiguration of replication or retention can cause data loss or storage bloat
- Complex dependencies between brokers, Zookeeper/KRaft, and clients can lead to cascading failures

## How Design Decisions Impact Scaling and Failures

Your choices in:

- Number of partitions and replication factor
- Consumer group architecture
- Deployment environment and hardware

...directly affect Kafka’s scalability, latency, and resilience.

In the following chapters, we’ll explore these design considerations with real-world examples and advice.

---

Mermaid Diagram (Kafka Consumer Group with Partition Assignment)
graph TD
    subgraph Kafka Topic
        P0[Partition 0]
        P1[Partition 1]
        P2[Partition 2]
        P3[Partition 3]
    end

    subgraph Consumer Group (group.id = my-consumer-group)
        C0[Consumer 0]
        C1[Consumer 1]
        C2[Consumer 2]
        C3[Consumer 3]
    end

    P0 --> C0
    P1 --> C1
    P2 --> C2
    P3 --> C3

🔄 Scalable Setup Explanation

Partitions: Represent logical divisions of a topic. Kafka guarantees message ordering within a partition.

Consumers: Multiple instances of the same application, each handling different partitions.

Consumer Group: Ensures each partition is read by only one consumer at a time.

Scalability: You can increase the number of partitions to scale out your consumers horizontally.

Ready to plan your production Kafka deployment? Let’s move on.

