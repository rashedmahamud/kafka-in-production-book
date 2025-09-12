12-kafka-cheat-sheets.md
# Chapter 12: Kafka Cheat Sheets

Quick references for common Kafka commands, configs, and monitoring tips.

## CLI Commands

- List topics:
  ```bash
  kafka-topics.sh --list --bootstrap-server <broker>:9092


Describe topic:

kafka-topics.sh --describe --topic <topic_name> --bootstrap-server <broker>:9092


Create topic:

kafka-topics.sh --create --topic <topic_name> --partitions <num> --replication-factor <num> --bootstrap-server <broker>:9092


Produce messages:

kafka-console-producer.sh --topic <topic_name> --bootstrap-server <broker>:9092


Consume messages:

kafka-console-consumer.sh --topic <topic_name> --from-beginning --bootstrap-server <broker>:9092

Config Properties Templates
# Broker config
log.dirs=/var/lib/kafka/logs
num.network.threads=3
num.io.threads=8
log.retention.hours=168
log.segment.bytes=1073741824
log.retention.check.interval.ms=300000
default.replication.factor=3
min.insync.replicas=2

# Producer config
acks=all
enable.idempotence=true
retries=5
linger.ms=20
batch.size=32768

Monitoring Dashboards

Use jmx_exporter for Kafka JMX metrics.

Import community Grafana dashboards (e.g., Kafka Overview, Consumer Lag).

Key metrics: under-replicated partitions, ISR count, consumer lag, broker CPU and GC.

Keep this cheat sheet handy for everyday Kafka ops.
