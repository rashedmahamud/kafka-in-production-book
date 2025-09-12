
# Chapter 11: Real-World War Stories

Learning from failures can save you from making the same mistakes. Here are some real Kafka production stories:

## That Time We Lost All Messages in a Topic

- A misconfigured retention policy combined with aggressive log compaction caused data to be deleted prematurely.
- Lesson: Always double-check retention settings and monitor disk usage closely.

## How Consumer Lag Almost Took Down the Pipeline

- Slow consumers caused backlog to build up, eventually filling broker storage and triggering throttling.
- Lesson: Monitor consumer lag and set alerts to catch slowdowns early.

## The Schema Mistake That Broke Everything

- A breaking schema change was pushed without proper compatibility checks, causing consumers to crash.
- Lesson: Use Schema Registry and enforce compatibility policies rigorously.

## Lessons from Moving to Confluent/MSK

- Migration to managed Kafka reduced operational burden but revealed hidden costs and feature gaps.
- Lesson: Test managed services thoroughly before committing.

---

These stories highlight why production Kafka requires vigilance and care.

---

Shall I continue with Chapter 12?
