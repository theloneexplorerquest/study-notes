why we distribute?
1. scalablity
2. fault tolerance/high availablity
3. latency
   
scaling to higher load: the cost grow faster than linearly

shared-nothing architecture: no special hardware is required, you can distribute the system across multiple regions. They requires most cautions. 

All the difficulty of replication comes from the need to changes in data.

Master-slave:
1. Leader: when the client want to write to the database.
2. follower: the leader also send the data change to all of its follower. Follower can serve read.

Synchonous vs asynchronous
1. sync: follower have up-to-date copy of data, however must block all writes.
2. aync: write is not durable, fast
3. semi-sync: one sync, other aync.

# Setting up new follower
