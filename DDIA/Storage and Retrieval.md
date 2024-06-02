Goal: you do need to select a storage engine that is appropriate for your application. In particular, there is a big difference between storage engine that optimised for transational workload and optimised for analytics.

# Data Structure That Power Your Database
Simple bash functions: db_set() have very good performance, db_get() is O(N) which is bad. To fix this, we can have a different data structure: an index (keep some additional metadata on the side as signpost to help locate the data)
Index only affect performance of the queries. For write, adding index slow down because index needs to be updated. This is a trade off, so developer need to choose indexes manually.
## Hash Indexes
Keep a in-memory hash map where each key is mapped to a byte offset. This is suitable to situations where the value is updated a lot, but not many keys in total. \
Prevent running out of disk space: break log into segments and write to new segment file when we reach certain limit. We can use compaction to throw away duplicate keys in the each segments. So segment become smaller (and we will have less segment), merged segment is written to a new file, the merging and compaction can be done in the background. (switich endpoint when merge is done). \
Each segment has its own hash table, to find a key, we check the newest segment to oldest.
Some issues:
1. Deleting record need to use tombstone. The record is deleted in the merge process.
2. Crash recovery: if the database is restarted, we can restore hash map by reading the entire segments one by one, which is slow. Can store the hashmap snapshot in disk.
3. partially written records: checksum.
4. Concurrency control: write is sequentail, read can be concurrent.\ 
Advantage: sequential write operations are fast, concurrency and crash recovery is simple
Disadvantage: table need to be memory, Range query is not efficent
## SSTable and LSM-Trees
The sequence of key-value is sorted by key and each key is only appearonce within each merged segment file.
Advantages:
1. merge is more efficient (can ): like mergesort, start by reading one by one in each segment file and input the smallest key, for same key keep the one from most recent segment file.
2. to find a key, there is no need to keep an index of all keys, jumping between the neighbor which have offset and scan until find the key (can be that the key is not in the file so need to go to another compaction file). So the key can be sparse, one key for every kb of segment file, scanning is fast for kb.
3. since read  request need to scan, we can group records and compress before writing to disk. save disk space and reduce IO bandwidth.
Constructing SSTables:
1. write incoming write to memtable (eg. AVL tree)
2. when the memtable is too big write it to disk as SSTable file (new thread).
3. read try to find key in memtable first, then on disk SSTable file.
4. run compaction.\
If database crash, the most recent write in memtable is lost. To resolve this, we keep a separate log which is not sorted. Its purpose is to restore the memtable. After memtable is wrtten to SST, the log is discarded.\
This is called LST table.
### Performance optimization
The LSM algorithm can be slow when looking up keys that does not exist. We use additional Bloom filters (can tell that a key is NOT in segment file, and a key is possibly in a segments) \
Merge strategies: 
1. size-tiered: newer and smaller SSTable are merged into older and larger SSTable.
2. leveled: key range is split up into smaller SSTable and older data is moved in the separate "levels".

## B-tree 

