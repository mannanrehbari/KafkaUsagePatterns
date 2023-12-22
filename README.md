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
<br>

---

<br>


### Pattern 2: Topic with a single partition and multiple consumers

If different business units need to process a given message differently, they can create their own consumers. All consumers subscribed to the topic will reeive all messages.
![Single Partition- Multiple Consumers](img/SinglePartition_MultipleConsumers.gif)


<br>
<br>

---

<br>

### Pattern 3: Topic with a single partition and single consumer group

Consumers can join a 'Consumer Group'. When multiple consumers are within a group, load is distributed across the members of the group. However, when there is a single partition for a given topic, all messages from the topic will be read a single member of the consumer group. 
The default 'Message Key' for kafka messages is Null. Kafka Producer puts messages with same key to same partition. Null is treated as a valid key.   

![Single Partition - Single Consumer Group](img/SinglePartition_SingleConsumerGroup.gif)


<br>
<br>

---

<br>

### Pattern 4: Topic with a single partition and multiple consumer groups

Messages will be forwarded to all consumer groups from the single partition. Each consumer group will the same behavior as described in Pattern 3 above. 
![Single Partition - Multiple Consumer Groups](img/SinglePartition_MultipleConsumerGroups.gif)


<br>
<br>

---

<br>

### Pattern 5: Topic with multiple partitions and a single consumer

Even though the topic has multiple partitions in this case, all messages will still end up in the same parition. The reason is default message key 'Null'. 

![Multiple Partitions - Single Consumer](img/MultiplePartitions_SingleConsumer.gif)


<br>
<br>

---

<br>

### Pattern 6: Topic with multiple partitions and multiple consumers

Since the message key is still not specified, Producer will append the message to a single partition.
Each subscribing consumer will receive a copy of the message from the broker. 

![Multiple Partitions - Multiple Consumers](img/MultiplePartitions_MultipleConsumers.gif)

<br>
<br>

---

<br>

### Pattern 7: Topic with multiple partitions and single consumer group
Multiple consumers within a consumer group CANNOT read from a single partition. However, the reverse is possible. A given consumer CAN read from multiple partitions. 
![Multiple Partitions - Single Consumer Group](img/MultiplePartitions_SingleConsumerGroup.gif)


<br>
<br>

---

<br>

### Pattern 8: Topic with multiple partitions and multiple consumer groups
In this case, we have 2 consumer groups. Consumer-group1 has two members and Consumer-group-2 has a single member. Within group 1, only Consumer-B is receiving all the messages. The reason is same as I described in previous patterns: consumers WITHIN a group read from specific partitions exclusively.

Within the second group, there is only a single consumer so it will receive all the messages on behalf of the group.
![Multiple Partitions - Multiple Consumer Groups](img/MultiplePartitions_MultipleConsumerGroups.gif)


<br>
<br>

---

<br>

### Pattern 9: Topic with multiple partitions, messages with distinct keys, and a single consumer
In this pattern, we are assigning distinct keys to Kafka messages. Distinct keys are color coded in this GIF. Kafka producer guarantees that messages with same key will be appended to the same partition.
However, it is also possible that each partition will have messages with different keys. But it is not possible that a new message with Blue Key will be appended to a different partition after the first blue message is added to partition-0.

There is only a single consumer in this example so it will read messages from all the partitions. 

![Message with distinct keys](img/Messages_Distinct_Keys.gif)


<br>
<br>

---

<br>

### Pattern 10: Topic with multiple partitions, messages with distinct keys, and multiple consumers

When messages are distributed across partitions, and there are consumers which are NOT part of a consumer group, they will receive all messages from all partitions. The messages are usually read from partition as soon as they are appended. It is possible to play with consumer offsets to change this behavior. 

![Messages with distinct keys, multiple consumers](img/MessagesDistinctKeys_MultipleConsumers.gif)


<br>
<br>

---

<br>

### Pattern 11: Topic with multiple partitions, messages with distinct keys, and a single consumer group

When consumers are part of a group, they will load balance from different partitions. Each member of the consumer group will read from 1 or more partitions. However, each partition will be read exclusively by a specific consumer. 

![Messages with distinct keys, single consumer group](img/MessagesDistinctKeys_SingleConsumerGroup.gif)