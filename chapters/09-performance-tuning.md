
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

- Increase Partitions: First, ensure your Kafka topic has more partitions than the number of consumers you intend to run in parallel. A common practice is to have a number of partitions equal to the desired concurrency level. If the number of consumers exceeds the number of partitions, some consumers will be idle.

- Run Multiple Consumers: Launch multiple instances of your consumer application, all configured with the same group.id.

- Automatic Partition Assignment: When these consumers start, they will join the consumer group. The Kafka broker and the consumer group protocol will automatically handle the assignment of partitions among the available consumers. For example, if you have a topic with 4 partitions and you start 4 consumer instances, each instance will be assigned one partition.

- Independent Processing: Each consumer instance will then independently and concurrently consume messages from its assigned partition(s). Since each consumer is running on a different thread or even a different machine, they can process messages from their respective partitions in parallel.

Implementation
## There are two main ways to implement this in code:

- Multiple Threads: In a single application, you can create and run multiple instances of your BackgroundService class on separate threads. Each BackgroundService instance would be a unique Kafka consumer instance with the same group.id.

- Multiple Processes: The more common and robust approach is to deploy your application as multiple, independent processes (e.g., in a Docker container or on separate virtual machines). Each process runs one consumer instance. This provides better fault tolerance and resource isolation.

- You do not use Task.Run on a single consumer.Consume call to achieve true parallelism across partitions. The Task.Run pattern is for achieving concurrency by offloading blocking calls, not for parallel message consumption across different partitions. To consume in parallel, you must have separate, independent consumer instances reading from different partitions.

  ## Errors at performance
   1. Error consuming message: "Application maximum poll interval (600000ms) exceeded by 198ms"
      - The error message "Application maximum poll interval (600000ms) exceeded" means that your consumer application took too long to call the Consume() method. The MaxPollIntervalMs in your configuration is set to 300000 (5 minutes). The error message shows the maximum interval was actually 600000 (10 minutes). This indicates a discrepancy between your code and your Kafka broker's configuration, which is the default 600000ms for max.poll.interval.ms. Your application is taking longer than 10 minutes (by 198ms) to call Consume().

## Why This Happens
- The max.poll.interval.ms setting defines the maximum amount of time a consumer can go without fetching records from its assigned partitions or sending a heartbeat to the broker. If this time is exceeded, the broker assumes the consumer is dead and will reassign its partitions to another consumer in the same group. This can lead to a rebalance.

## This issue is typically caused by:

- Long-Running Message Processing: The code that processes the message after Consume() is called is taking too long. Since you have await ProcessWithRetriesAsync(...) inside the loop, the main loop is paused while that method runs. If ProcessWithRetriesAsync takes longer than the max.poll.interval.ms, this error will occur.

- Awaiting Blocking Calls: Similar to the previous issue, if ProcessWithRetriesAsync or any other code within your consumption loop is blocking a thread, it can prevent the next Consume() call in the loop from happening in time.

## How to Fix It
- To resolve this error, you must ensure your message processing logic completes within the max.poll.interval.ms limit. The best way to do this is to separate the consumption loop from the message processing logic.

- Increase MaxPollIntervalMs (Temporary Solution): You can increase the MaxPollIntervalMs in your ConsumerConfig to match the time your processing takes. However, this is not a long-term solution. A large interval can delay partition rebalancing in the event of a real consumer failure.

- Separate Consumption and Processing (Recommended): The correct approach is to put the message processing on a separate task or thread. This allows your consumer loop to poll for new messages quickly, even if a previous message is still being processed.

-Here's a refactored code example that demonstrates this pattern:

```csharp
// Inside your ExecuteAsync loop
while (!stoppingToken.IsCancellationRequested)
{
    try
    {
        var consumeResult = await Task.Run(() => consumer.Consume(TimeSpan.FromMilliseconds(100)), stoppingToken);

        if (consumeResult?.Message is not null)
        {
            // Offload the message processing to a new task.
            // Do not 'await' this task, so the consumer loop can continue.
            _ = Task.Run(async () =>
            {
                await ProcessAndCommitAsync(consumer, consumeResult, scope, kafkaProducerService, stoppingToken);
            });
        }
    }
    catch (ConsumeException ex)
    {
        _logger.LogError(ex, "Error consuming message: {Reason}", ex.Error.Reason);
        await Task.Delay(1000, stoppingToken);
    }
}

// New method to encapsulate the processing and committing logic
private async Task ProcessAndCommitAsync(IConsumer<string, string> consumer, ConsumeResult<string, string> consumeResult,
    IServiceScope scope, IKafkaProducerService kafkaProducerService, CancellationToken stoppingToken)
{
    if (_topicHandlers.TryGetValue(consumeResult.Topic, out var handler))
    {
        var isProcessed = await ProcessWithRetriesAsync(
            consumeResult,
            handler,
            scope,
            kafkaProducerService,
            stoppingToken,
            Convert.ToInt32(_configuration["KafkaConfig:MaxRetryAttempts"]),
            Convert.ToInt32(_configuration["KafkaConfig:RetryDelayMs"])
        );

        if (isProcessed)
        {
            consumer.Commit(consumeResult);
            _logger.LogInformation("Message successfully processed and committed.");
        }
    }
}
```


- This pattern ensures the consumer.Consume() method is called frequently (every 100ms or when a message is available), which keeps the heartbeat alive and prevents the max.poll.interval.ms from being exceeded. The actual message processing runs in the background. .


