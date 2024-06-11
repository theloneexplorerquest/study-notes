Data-Intensive: CPU power is not a limiting factor, bigger problems are the amount of data, the complexity of data, and the speed of changing.
Typically components: databases, caches, search indexes, streaming processing, and batch processing. \
# Thinking about Data Systems
Why we unify databases, queries, caches etc as data systems:
1. Many new tools for data storage and processing have emerged.
2. For many applications, a single tool can not longer meet all of its need. Example: application with cache and database usually hide implementation details from clients.
Question to ask myself:
1. how to ensure data correctness and completeness (when things go wrong)
2. how to provide good performance to clients (even part of the system are degraded)
3. how to scale to handle increase in load
4. API design.
This book is about:
1. Reliability: the system should work even in face of adversity
2. Scalablity: as the system grow the system should handle the load.
3. Maintainablity: people work on the system should be able to work on it productively.
# Reliablity
Continue to work correctly, even when things go wrong.\
Fault is different from failure: Fault is one component of the system deviating from spec, failure is the system as a whole stops working. In such fault-toleant system, we can increase the rate of fault by triggering them deliberately: we can ensure that the fault -tolerance machinery is continually exercises and tested.
In general, tolerating faults is better than preventing faults.

## Hardware Faults
1. add redundancy to individual hardware components to reduce the failure rate. Example: Disk, dual power supplier, battery for datacenters. It is well-understood.
2. As the data volume and computing demand has increased, more application starts to use larger numbers of machine, which increase the rate of hardwares faults. So we use software fault-toleance.

## Software Error:
Hard to anticipate, correlated across the nodes, they tend to cause many more system failures. They often lie dormant for a long time until they are triggered in unususal set of circumstances. No quick solutions: 1. think about assumptions and interactions, thorough testing, process isolation, allowing processes to creash and restart, measuring, monitoring and analyzing system behavior in productions.

## Human Errors:
1. Design systems in a way that minimize opportunities for error. API, admin interfaces ..
2. Decouple the place people make the most mistakes from the place they can cause failures. sandbox environment.
3. Test thoroughly at all levels: unit test, integration tests and manual tests.
4. Allow quick recovery: rollback configuration changes, roll out new code gradually.
5. Setup monitoring: performance metrics and error rates.
6. Good management practice and training.
Reliablity is important, but we may choose to sacrifice reliablity to reduce developement cost.

# Scalability
Describe system's ablity to cope with increased load. Not one-dimensional: if a system grows in a particular way, what is our options?
## Describing Load
It depends on the architecture of the system: request per seond to web server, ratio of read to write in database.
Twitter as an example, have two main operations: Post Tweet and Home timeline. The bottleneck is at fanout:
1. Post a sweet inserts the new tweet into global collections. When user request home timeline, lookup all people follow, find all tweets for each of the other and merge.
2. Maintain a cache for each user's home timeline. When a user post tweet, look up all people who follow that user, and insert new tweet into home timeline caches. Computed ahead of time. \
The second is better because the average rate of published tweets is two orders of magnitude lower than home timeline reads. However, the downside is posting a tweet new requires a lot of extra. (some stats). The distribution of followers per uers is a key load parameter for discussing scalablity. Twiter use both mode.

## Describe Performance
Two ways:
1. system resource unchanged, increase the load, how the performance is affected?
2. performance unchanged, increase the load, how much we need to increase the resources?\
Throughput: the number of records we can process per second (Hadoop)\
Response Time: online system, the time between a client sending a request and receiving a reponse.\

To measure response time, it is better to use percentiles instead of average since it can tell how many users actually experience that delay. Median is good as well (p50). To get how bad the outlier are: 95th, 99th and 99.9th -> tail latencies. \

The slowest request are often those who have the most data on accounts (most valuable customers). However, 99.99th can be too expensive, and it is difficult and can be the things out of your control, the benefits is diminishing. \

Queueing delays often account for a large part of the response, it only take a small numbers of slow requests to hold up the processing. (head-of-line blocking). Even if the subsequent request are fast the overall response time is fast. It is important to measure response time on client end. When testing the scalablity, client need to keep sending request independently. If wait, we artifically keep the queue shorter. 

   

