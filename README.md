# Common Usage Patterns of Apache Kafka


<br>
Apache Kafka is an open-source stream processing platform known for handling large volumes of data with high throughput and low latency. It operates on Pub-Sub model and is suitable for building real-time pipelines and streaming applications. This repository contains some basic patterns related to usage of Apache Kafka. I created some animated GIFs for these patterns since it can a bit complex to visualize from sources such as text, code, and even static images. This repository will evolve as I explore Kafka in more detail. 

<br>
<br>

---

<br>

### Pattern 1: Topic with a single partition and a single Consumer

This is the simplest possible use of Kafka. The producer receives messages from any source, and creates a time-ordered log with the received messages. In this case the single consumer will receive messages from the partition and process it. 

![Single Partition-Single Consumer](img/SinglePartition_SingleConsumer.gif)


<br>


### Pattern 2: Topic with a single partition and multiple consumers

If different business units need to process a given message differently, they can create their own consumers. All consumers subscribed to the topic will reeive all messages.
![Single Partition- Multiple Consumers](img/SinglePartition_MultipleConsumers.gif)


<br>

### Pattern 3: Topic with a single partition and single consumer group

Consumers can join a 'Consumer Group'. When multiple consumers are within a group, load is distributed across the members of the group. However, when there is a single partition for a given topic, all messages from the topic will be read a single member of the consumer group. 
The default 'Message Key' for kafka messages is Null. Kafka Producer puts messages with same key to same partition. Null is treated as a valid key.   

![Single Partition - Single Consumer Group](img/SinglePartition_SingleConsumerGroup.gif)


<br>

### Pattern 4: Topic with a single partition and multiple consumer groups

Messages will be forwarded to all consumer groups from the single partition. Each consumer group will the same behavior as described in Pattern 3 above. 
![Single Partition - Multiple Consumer Groups](img/SinglePartition_MultipleConsumerGroups.gif)


<br>

### Pattern 5: Topic with multiple partitions and a single consumer

Even though the topic has multiple partitions in this case, all messages will still end up in the same parition. The reason is default message key 'Null'. 

![Multiple Partitions - Single Consumer](img/MultiplePartitions_SingleConsumer.gif)


<br>

### Pattern 6: Topic with multiple partitions and multiple consumers

Since the message key is still not specified, Producer will append the message to a single partition.
Each subscribing consumer will receive a copy of the message from the broker. 

![Multiple Partitions - Multiple Consumers](img/MultiplePartitions_MultipleConsumers.gif)

<br>

### Pattern 7: Topic with multiple partitions and single consumer group
Multiple consumers within a consumer group CANNOT read from a single partition. However, the reverse is possible. A given consumer CAN read from multiple partitions. 
![Multiple Partitions - Single Consumer Group](img/MultiplePartitions_SingleConsumerGroup.gif)


<br>

### Pattern 8: Topic with multiple partitions and multiple consumer groups

![Multiple Partitions - Multiple Consumer Groups](img/MultiplePartitions_MultipleConsumerGroups.gif)


<br>

### Pattern 9: Topic with multiple partitions, messages with distinct keys, and a single consumer

![Message with distinct keys](img/Messages_Distinct_Keys.gif)


<br>

### Pattern 10: Topic with multiple partitions, messages with distinct keys, and multiple consumers
![Messages with distinct keys, multiple consumers](img/MessagesDistinctKeys_MultipleConsumers.gif)


<br>

### Pattern 11: Topic with multiple partitions, messages with distinct keys, and a single consumer group
![Messages with distinct keys, single consumer group](img/MessagesDistinctKeys_SingleConsumerGroup.gif)