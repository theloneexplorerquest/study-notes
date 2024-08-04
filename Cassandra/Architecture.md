1. cassandra use data center and racks. While preferring to route queries to nodes in the local data center to maximize performance.
2. Cassandra uses a gossip protocol that allows each node to keep track of state information about the other nodes in the cluster.  the failure monitoring system outputs a continuous level of “suspicion” regarding how confident it is that a node has failed
3. snitch: provider infromation about network topology so Cassanbdra can route quess, figure out node relations.
4. Cassandra represents the data managed by a cluster as a ring.
5. A partitioner determines how data is distributed across the nodes in the cluster.
6. replication strategy: The first replica will always be the node that claims the range in which the token falls. 
