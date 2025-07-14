# Event-Driven Architecture

Terminology:
* **event**: Small, self-contained, immutable object containing the details of something that happened at some point in time.
* **producer**: Generates the event.
* **consumer**: Processes the event (can be more than one)

Messaging patterns:
* Direct (single consumer)
* Load balancing (multiple consumers): Multiple consumers, but only one processes the message.
* Fan-out (multiple consumers): All subscribed consumers process the message.

Connecting producer and consumer:
* _Direct communication (TCP, webhooks...)_: This is not the best option, because it requires both producer and consumer to be alive in order for event to be sent, otherwise some events might be lost. Also, what happens if producer sends messages faster than a consumer can process them?
* _Database_: This is also not the best option because it requires consumer to poll the datastore, which can cause an overhead. Although database have triggers, they are very limited in what they can do.
* **_Message broker (akka message queue)_**: Specialized tools developed for the purpose of delivering events. They are (usually) durable, and can tolerate consumer outages.

Broker types:
* **JMS/AMPQ** (traditional): Messages get deleted after the consumer acknowledges that it has processed it. On the other hand, messages reappear if the consumer doesn't acknowledge processing.
    * Applications: Google Cloud Pub/Sub
    * Cons:
        * Reappearing of messages can affect the ordering of the messages.

* **Log-based**: A log-structured, append-only, sequence of record stored on disk (like databases). Producer sends a message by appending it to the end of the log, and a consumer receives messages by reading the log sequentially. If a consumer reaches the end of the log, it waits for a notification that a new message has been appended. Records are never deleted or updated, each consumer keeps track of its sequence number which represents the last processed message. When a new consumer joins, it can start consuming events from the moment of joining, but it can also be configured to start consuming events from beginning.
    * Applications: Apacke Kafka, Amazon Kinesis, DistributedLog
    * Cons:
        * Not trivial to support _load balancing_ messaging pattern (making sure that each message is processed by only one consumer), because the messages are not deleted, and the broker doesn't keep track of consumers. We can only achieve a certain level of load balancing by assigning entire partitions to clients, which means that we cannot have more consumers than partitions.
