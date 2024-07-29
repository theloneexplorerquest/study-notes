https://www.hellointerview.com/learn/system-design/in-a-hurry/core-concepts
# Basic
Scaling: adding machine isn't a free lunch, vertical scaling is easier, need to consider the implication of horizontal scaling. Horizontal scaling involves sync challenge (need to discuss how to keep data consistent), may need a distributed lock.

Work Distribution: load balancer, for async this is done via queueing system.

Data Distrbution: way to partition data such a single node can access the data without needing to talk to another node, except for fan out. Good to have some region id.

Consistency: banking system. Many system blend strong and weak consistency in different part. Even if you use a strong consistent database, inserting a chache will make it eventual consistency.

Locking: we might have shared resources which can be only accessed by one client at a time, locking is the process of ensuring that only one client cana access a shared resources at a time. Need this hen race condition. We want to lock as little, short as possible, can use optimistic concurrency control strategy.

Indexing: make data query faster. Beside hash index, we can keep  data in a sorted list. Fro database like Redis your are on your own to design indexing stragy. Specialised Index: geospatial indexes are used to index location data. Can use elasticSearch (point of failure and latency and stale data).

Communication Protocol: Internal it is easier HTTPs or gRPC. externally: HTTPs SSE, long polling and Websockets. HTTPs for API with simple request and response, stateless so can scale horizontally behind LB (not dependent on sessions). near realtime updates, long polling, LB is working. Websocket is for realtime and challenge for LB and firewall, challenge for server to maintain many connections. we can use a message broker to handle the communcation (how?). SSE

Security: should be top of mind. API gateway will handle the auth.  data in transit (SSL/TLS) and data in rest (storage encryption). Give ebnd user the key to user data, might want to encrpt data with a key unique to each user. rate limiting.

Monitoring: Infrastructure Monitoring: CPU, memory, disk and network. Service-level Moniroting: request latency, error rates, throughput. 

Application-level monitoring: number of user, number activesessions, active connections. Key business metrics for the business. GoogleAnlytics.




