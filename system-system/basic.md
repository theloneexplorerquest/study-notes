https://www.hellointerview.com/learn/system-design/in-a-hurry/core-concepts
# Basic
Scaling: adding machine isn't a free lunch, vertical scaling is easier, need to consider the implication of horizontal scaling. Horizontal scaling involves sync challenge (need to discuss how to keep data consistent), may need a distributed lock.

Work Distribution: load balancer, for async this is done via queueing system.

Data Distrbution: way to partition data such a single node can access the data without needing to talk to another node, except for fan out. Good to have some region id.

Consistency: banking system. Many system blend strong and weak consistency in different part. Even if you use a strong consistent database, inserting a chache will make it eventual consistency.

Locking


