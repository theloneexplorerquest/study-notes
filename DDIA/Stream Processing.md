# Chapter 10 Batch Processing
## Transmitting Event Streams
### Messaging Systems:
message broker compared to databases:
1. data rentation
2. data size
3. database support secondary index, while broker support subscribe a subset of topic matching a pattern
4. query in database is snapshot of the data; broker do not support arbitrary queries, but notify changes.
#### multiple consumers:
1. load balancing: deliver to one consumer, consumers share all messages. When message are expensive to process.
2. fan-out: deliver to all consumers, broadcasting
Two can combined.
#### acknowledgements and redelivery
A client must explicitly tell the broker when it has finished processing a message. Acknowledgements could be lost when the message was processed, need automic commit. Also, the order is not preserved (with load balancer). \
### Partitioned Logs:
Transient messaging mindset: send a packet or making a request to network service have no trace.\
The oppsite: database and filesystems: everything is permanently recorded. \
1. batch processing: run repeatedly without demaging the input.
2. AMQP messaging: receiving message is destructive.

Hybrid approach: log-based message brokers.
To scale the higher throughput: we can partition log and host on different machine. Topic is defined as a group of partition carry messages of the same type.
reading in the same parition is order-guaranteed since it is append only.
## Database and Streams
### keeping System in Sync
### Change data Capture
