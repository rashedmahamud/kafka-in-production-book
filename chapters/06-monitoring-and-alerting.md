
# Chapter 6: Monitoring & Alerting

Keeping Kafka healthy in production requires robust monitoring and meaningful alerts.

## Key Metrics to Watch

- **Broker Metrics:** disk usage, network throughput, CPU, JVM heap, garbage collection
- **Partition Metrics:** in-sync replicas (ISR), under-replicated partitions (URP), leader election rate
- **Consumer Metrics:** consumer lag, group rebalances, fetch and commit latencies
- **Producer Metrics:** request latency, retries, error rates

## Using Prometheus + Grafana

- Prometheus can scrape Kafka JMX metrics with exporters like `jmx_exporter`.
- Grafana dashboards provide visualizations for cluster health and trends.
- Use community dashboards as a starting point and customize to your needs.

## Setting Up Alerts That Matter

- Alert on under-replicated partitions or ISR changes — early signs of issues.
- Watch consumer lag thresholds to detect slow consumers.
- Set alerts on broker resource exhaustion (disk, CPU, memory).
- Avoid noisy alerts by tuning thresholds and using alert suppression for maintenance windows.

---

Effective monitoring and alerting help you catch issues before they impact users.

---

Ready for Chapter 7?
