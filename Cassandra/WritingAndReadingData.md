1. tunrable consistency level.
write path:
1. a client init a write query to coorinator.
2. identify which node has relicas
3. send write request to all replicas. can be repair with hinted handoff, read repair, or anti-entropy repair.
4. receive the write request and immediately write to commit log, memtable. after return, the node flush contents to sstable and clear commit log. check compaction is needed.
5. not ACID: lightweight transation. Paxos algorithm
6. support batching

Reading
1. slower because file I/O in SST, can use cache
2. consistency level, will perform read repair before or after return to client
read path:
1. client init a read query.
2. calculate hash to determine if consistent.
3. on node is first check row cache-> memtable, sstable (bloom filter)
