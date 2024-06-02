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
