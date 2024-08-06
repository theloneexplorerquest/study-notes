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
4. run compaction.
## Constructing and maintining SSTables
If database crash, the most recent write in memtable is lost. To resolve this, we keep a separate log which is not sorted. Its purpose is to restore the memtable. After memtable is wrtten to SST, the log is discarded.\
This is called LST table.
### Performance optimization
The LSM algorithm can be slow when looking up keys that does not exist. We use additional Bloom filters (can tell that a key is NOT in segment file, and a key is possibly in a segments) \
Merge strategies: 
1. size-tiered: newer and smaller SSTable are merged into older and larger SSTable.
2. leveled: key range is split up into smaller SSTable and older data is moved in the separate "levels".

## B-tree 
B-tree break the database down into fixed-size blocks or pages (more align with hardware as disk is fixed-size block)

Each page can be identified using an address or location on disk.

leaf page: a page contains individual keys (inline or reference to the page where the value can be found).

Add a new key: find the page and add to that page, if there is not enough space, we need to split into two pages.

B-tree is balanced, depth O(log n)

### make B-tree reliable
LSM tree never modify the file in place, B-tree over-write. 

Some operation requires several different pages to be overwritten (split the parent when insertion is full): dangerous because database crashes after some of the page has been written. Solution is to have a WAL to use when crash,

concurrency can be a problem as well: use latch (LSM was easier)

### optimisation
1. copy-on-write instead of WAL and concurrency.
2. store abbreviating of a key (boundary information is all we need).
3. additional pointer for the tree so leaf can go to sibling without jumping back to parents

## Comparing B-tree and LSM-trees
1. read: B-tree>LSM tree (the need to check different ds)
2. wrote: LSM-tree>B-tree
### advantage of LSM
1. B-tree need to write data twice, also need to write an entire page even a few bytes changed.
2. LSM write data multiple times due to compaction- write amplification
3. write amplifcation: LSM< B-tree. sequentially write SST is faster than random write. So SST write throught is better.
4. LSM is smaller, B-tree leave disk space unused due to fragmentation.
### Downside of LSM-tree
1. compaction can interfere on going performance and read and write: a request need to wait while the disk finish an expensive compaction.
2. The disk finite write bandwidth needs to be shared between initial write and compaction thread. It could happen that compaction cannot keep up with incoming write. 
3. B-tree is good because each key is in one place: strong transactional semantics.
## Other index structure
k-v index are like primary index in relational database. It is very common to have secondary indexes. Secondary index is not unique. There might be many rows with the same key: either make each value in the index a list of matcing row identifiers.  

# Transaction Processing or Analytics
A transaction needn't necessayily have ACID.

OLAP: Database also started being increasingly used for data analytics: scan over a huge number of records, only read a few columns per record, abd calculate the aggregate. These data are for data intelligence.

## Data Warehousing
An enterprise may have different tansaction processing systems. The OLAP system are expected to be highly available and process transactions with low latency. Running analytic queries on OLAP are expensive and can harm the performance of concurrently excutation.

The datawarehouse contains read-only copy of the data in OLTP system -> ETL. Data can be optimised for analytic access pattern. The data model for data warehouse is commonly relational because SQL is a good fit for analytic queries. However ther internal is different.

## Star and Snowflakes: Schema for Analytics.
There are less diversity of data models: most of them use star schema. Fact table: fact are captured as individual events, because it allows maximum flexiblity., but it also means to fact table can be extremely large. Some are attributes, so are foreign keys reference to other tables, called dimension tables: represent who, what, where, when, how, and why of the event. Event date and time are often represented using dimension tables because this allow additional informations.

A variation of this is called snowflake: each dimension if further broken down into subdimensions. It is more normalised than star schema.

## Column-Oriented Storage
Storing and querying data can be a major problem in fact tables. A typical query only use 4-5 of the column. The idea: don't store all the values from one row together, but store value from each column together instead. Because all columns are stoed in a separate files, a query only needs to read and parse those columns used in the query, which save a lot of work.
## Column compression
Often the number of distinct value in column is smaller than row, we can take a column and turn it into n sperated bitmaps. 
## Sort order in column storage
