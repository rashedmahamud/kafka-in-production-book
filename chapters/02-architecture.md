
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
<details> <summary>✅ Click to copy valid Mermaid markdown</summary>
```mermaid
graph TD
    subgraph Kafka_Topic
        P0[Partition 0]
        P1[Partition 1]
        P2[Partition 2]
        P3[Partition 3]
    end

    subgraph Consumer_Group_my_consumer_group
        C0[Consumer 0]
        C1[Consumer 1]
        C2[Consumer 2]
        C3[Consumer 3]
    end

    P0 --> C0
    P1 --> C1
    P2 --> C2
    P3 --> C3


</details>

---

### 🔍 Notes:

- Avoid special characters like `(`, `)`, `=`, or `.` in **`subgraph` names**.
- Use underscores `_` or hyphens `-` instead.
- You can explain the `group.id = my-consumer-group` separately in a heading or caption.

---

### 📝 Optional README Markdown Example:

```markdown
## Kafka Consumer Group Partition Assignment

This diagram shows how Kafka assigns partitions to consumers within the same consumer group.

- Topic has 4 partitions.
- 4 consumers are started with the same `group.id`.
- Each consumer is automatically assigned one partition.

```mermaid
graph TD
    subgraph Kafka_Topic
        P0[Partition 0]
        P1[Partition 1]
        P2[Partition 2]
        P3[Partition 3]
    end

    subgraph Consumer_Group_my_consumer_group
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


diagram showing three cases:

Equal partitions and consumers (1:1 assignment)

More consumers than partitions (some consumers idle)

More partitions than consumers (some consumers handle multiple partitions)

Mermaid Diagram: Kafka Partition Assignment Edge Cases
```mermaid
graph TD
    %% Topic Partitions
    subgraph Kafka Topic
        P0[Partition 0]
        P1[Partition 1]
        P2[Partition 2]
        P3[Partition 3]
    end

    %% Case 1: Equal partitions and consumers (4 consumers, 4 partitions)
    subgraph "Case 1: Equal consumers and partitions"
        direction TB
        C0[Consumer 0]
        C1[Consumer 1]
        C2[Consumer 2]
        C3[Consumer 3]
    end

    P0 --> C0
    P1 --> C1
    P2 --> C2
    P3 --> C3

    %% Case 2: More consumers than partitions (6 consumers, 4 partitions)
    subgraph "Case 2: More consumers than partitions"
        direction TB
        C4[Consumer 0]
        C5[Consumer 1]
        C6[Consumer 2]
        C7[Consumer 3]
        C8[Consumer 4 (idle)]
        C9[Consumer 5 (idle)]
    end

    P0 --> C4
    P1 --> C5
    P2 --> C6
    P3 --> C7
    %% C8 and C9 are idle: no partitions assigned

    %% Case 3: More partitions than consumers (4 consumers, 6 partitions)
    subgraph Kafka Topic Extra
        P4[Partition 4]
        P5[Partition 5]
    end

    subgraph "Case 3: More partitions than consumers"
        direction TB
        C10[Consumer 0]
        C11[Consumer 1]
        C12[Consumer 2]
        C13[Consumer 3]
    end

    P0 --> C10
    P1 --> C10
    P2 --> C11
    P3 --> C11
    P4 --> C12
    P5 --> C13

Explanation:

Case 1: Each partition is assigned to exactly one consumer.

Case 2: With more consumers than partitions, some consumers (C8, C9) get no partitions and stay idle.

Case 3: With fewer consumers than partitions, some consumers handle multiple partitions (e.g., C10 handles P0 and P1).


Ready to plan your production Kafka deployment? Let’s move on.

