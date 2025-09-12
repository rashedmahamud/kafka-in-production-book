# Kafka in Production: Real-World Lessons from the Trenches

This is a practical, experience-based guide for engineers who want to run Apache Kafka **reliably and at scale** in production environments.

Whether you're deploying Kafka for the first time or you're already knee-deep in partition counts and consumer lag, this book captures the lessons, strategies, and mistakes from real-world Kafka deployments.

---

## 📘 What This Book Covers

- Deploying Kafka for production use
- Optimizing producers and consumers for scale
- Monitoring and alerting with the right metrics
- Handling outages and debugging real issues
- Avoiding common mistakes and architectural pitfalls
- Tuning Kafka for performance and reliability

---

## 🧑‍💻 Who This Book Is For

- Backend Engineers
- Platform / DevOps / SRE Teams
- Data Engineers
- Architects designing event-driven systems
- Anyone running Kafka clusters in production

---

## 📚 Table of Contents

### Part 1: Getting Kafka into Production

- 📖 **Chapter 1: Introduction – Why Kafka in Production?**
  - Why you wrote this book
  - Who this book is for
  - What Kafka solves
  - What readers will learn

- 📖 **Chapter 2: Kafka Architecture Refresher (Real-World Framing)**
  - Topics, partitions, brokers, Zookeeper/KRaft
  - Design strengths vs production challenges
  - Impact of design choices on scale and reliability

- 📖 **Chapter 3: Planning for Production**
  - Self-hosted vs Confluent Cloud vs MSK
  - Hardware vs SaaS decisions
  - High availability and partitioning strategies

- 📖 **Chapter 4: Setting Up Kafka the Right Way**
  - Docker, Kubernetes, bare metal
  - Common setup mistakes
  - Durability configs and benchmark testing

---

### Part 2: Operating Kafka in Production

- 📖 **Chapter 5: Producers and Consumers in the Real World**
  - Tuning throughput, retries, batching
  - Idempotent producers, delivery guarantees
  - Consumer groups, lag, rebalancing strategies

- 📖 **Chapter 6: Monitoring & Alerting**
  - Essential Kafka metrics (disk, GC, ISR, lag)
  - Prometheus + Grafana setups
  - Setting up actionable alerts

- 📖 **Chapter 7: Kafka Connect & Schema Registry**
  - Using Kafka Connect at scale
  - Managing schema evolution safely
  - Avoiding downstream data breakage

- 📖 **Chapter 8: Security in Production**
  - TLS, SASL, ACLs
  - Real-world auth issues
  - Network access control and hardening

---

### Part 3: Scaling & Failure Handling

- 📖 **Chapter 9: Performance Tuning at Scale**
  - Broker and JVM tuning
  - Partitioning strategies
  - Scaling the cluster

- 📖 **Chapter 10: Handling Outages & Recovering**
  - Broker failures and recovery
  - Zombie leaders, ISR shrinkage
  - Backups and data recovery

- 📖 **Chapter 11: Real-World War Stories**
  - “That time we lost all messages in a topic”
  - “How consumer lag almost took down the pipeline”
  - “The schema mistake that broke everything”
  - “Lessons from moving to Confluent/MSK”

---

### Part 4: Practical Resources

- 📖 **Chapter 12: Kafka Cheat Sheets**
  - CLI commands
  - Config templates
  - Monitoring dashboards

- 📖 **Chapter 13: Kafka Interview Questions & Scenarios**
  - Real interview questions
  - Questions to assess Kafka readiness in teams

- 📖 **Chapter 14: Tools & References**
  - Tools: kcat, kafkajs, MirrorMaker, cruise-control
  - Books, blogs, and community resources

---

## 🛠️ How to Use This Repo

This repo contains the source files of the book in Markdown format under the `/chapters` folder. You can:

- Read the chapters online
- Fork and contribute with improvements or suggestions
- Clone and use it as a template for your own Kafka playbook

---

## 📬 Contributions Welcome

Got your own Kafka war stories or fixes? Open a pull request or suggest an issue.

---

## 📜 License

This book is published under the [MIT License](LICENSE).

---

## 🔗 Stay Updated

Coming soon: Free PDF / GitBook version, newsletter signup, and more.

