why we distribute?
1. scalablity
2. fault tolerance/high availablity
3. latency
   
scaling to higher load: the cost grow faster than linearly

shared-nothing architecture: no special hardware is required, you can distribute the system across multiple regions. They requires most cautions. 

All the difficulty of replication comes from the need to changes in data.

# Leader and Follower
Master-slave:
1. Leader: when the client want to write to the database.
2. follower: the leader also send the data change to all of its follower. Follower can serve read.

Synchonous vs asynchronous
1. sync: follower have up-to-date copy of data, however must block all writes.
2. aync: write is not durable, fast.
3. semi-sync: one sync, other aync.

## Setting up new follower
1. take a consistent snapshot of the leader without taking a lock.
2. copy the snapshot to follower node.
3. follower connect to leader and request all data changes happened since snapshot.

# Handling Node Outages

Follwer failure: on the local disk, each follwer keeps a log of data ,follower can connect to the leader and request all the data changed. 

Leader failure: one of the follower needs to be promoted to be a new leader, can be automatic or manual, challenge:
1. determine the leader has failed: most system use timeout.
2. choose new leader: election process by all nodes or appointed by leader. The best candidate is the one with most up-to-date data changes. 
3. reconfigure the system use new leader.
Thing can go wrong:
1. for aync replication, new leader may not received all write from the old leader before failed. If the former leader rejoin and write. it will cause conflict. We can just disgard write, however violate durablity.
2. split brain: both leader accept write.
3. how to determine timeout.
Therefore some operation prefer to fail manually.

# Multi-Leader Replications
master/master or active/active replications.
use case:
1. multi-datacenter replications: in single-leader, write need to go through internet to thge datacentre with the leaders. slow. However in multi-leader config it is sync, network delay is hidden from users.
2. Tolerance of outages: each dc continues to operating independently, replicate catch up with failed cd come back.
3. Tolerance of network: not sensitive to Internet network.
Downside: write conflicts. Dengerous

client with offline operation: every device has a local database that act as a leader.

collaborative editing: the changes are instantly applied to local replications and sync replicated to the server. To guarantee there will be no conflicting, a lock is needed: small unit and avoid locking.

## Handling Write Conflicts:
sync vs aync detection: in single replication, the second write will either get blocked or wait to complete, or abort. In here both write are successful and conflict is detected async.

conflict avoidance: recommanded approach. eg, user can only edit their own data, request from same user always routed to the same dc. However sometimes a dc can fail or user moved to a different location.

converaging toward a consistent state: there is no defined ordering of writes. give each write a unique ID, last write win, data loss. relicate a unique ID. merge, CRDT 
 
