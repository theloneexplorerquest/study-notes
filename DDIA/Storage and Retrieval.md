Goal: you do need to select a storage engine that is appropriate for your application. In particular, there is a big difference between storage engine that optimised for transational workload and optimised for analytics.

# Data Structure That Power Your Database
Simple bash functions: db_set() have very good performance, db_get() is O(N) which is bad. To fix this, we can have a different data structure: an index (keep some additional metadata on the side as signpost to help locate the data)
Index only affect performance of the queries. For write, adding index slow down because index needs to be updated. This is a trade off, so developer need to choose indexes manually.
## Hash Indexes

