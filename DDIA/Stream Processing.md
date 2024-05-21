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
## Processing Streams
What to do once you have streamed data:
1. write it to database.
2. push notifications or sending emails.
3. produce more stream.

Different between batch process is stream never end, so sort-merge cannot be used. 
Fault-tolerance need to change as the job has been running for years so restart is not an option.\
### Uses of Stream Processing
Fraud detection, execute trades in trading systems, monitor the status of machines in factory.\
#### Complex event processing 
Analyzing event streams, search for event patterns. Use SQL or graphic UI queries to consumes the input stream and maintains a state machine. When there is a match, the engine emits a complex event. CEP reverse role of data and query: query are for long-term store, events from the input streams.
#### Stream analytics
Similar to CEP, less interested in finding specific event sequences and is more oriented towards aggregations and statistical metrics.
For example, rate of event, rolling average of value. Sometimes use probabilistic algorithm
#### maintaining materialised views
The application state is also a kind of materialized view, building materialised view requires all events over an arbitrary time period.
### Reasoning About Time
The batch processing has no need to look at timestamp as it has nothing to do with the time the event occurs. (deterministic) \
However, streaming processing use local clock: it is reasonable if the event creation and processing is negligibly short.\
#### Event time vs processing time
Reason to be delayed: queueing, network faults etc. Message can be also lead to unpredictable ordering of messages. 
