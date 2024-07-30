# Core Database
Product deisng interview take relational database, infrasturcture design take NoSQL database
NoSQL and SQL are highly overlapping: noSQL can work great for relation, SQL can scale and performance as well.
Truth: no need to explicit comparion of SQL and NoSQL, you should avoid it. Instead, talk about why you know about the databse and how it will help you solve the problem at hand.
For compare, focus on the differences in the database you are familar with and how they would impact your design.

# Relational Database
1. SQL Joins
2. Indexes (allow faster query, B-tree of HashTable), support for arbitrarily many indexes.
3. Transactions: way to group multiple operations together.

# NoSQL Databases
NoSQL databse do not use table based structyrue and are often schema-less: unstructured, semi-structured or strctured data:
1. flexible data models: data model is evovling or store data strcuture without a fixed schema.
2. scalablity: scale horizontally.
3. Application dealing with large volume of data. (time series etc.)
Should discuss the specifc feature of the database.
Should know:
1. data model: kv store, document store, column-family store, and graph database.
2. consistency models: from stron to eventual consistency.
3. indexing: B-tree of Hash Table.
4. Scalablity: sharding to distribute data.
# Blob Storage
Store large unstructured blobs of data, storing these in database is expensive and inefficient. upload data abd get back URL, use the URL to download the blob of data. Use Blob as origin and use CDN to cache the file/blob in edge locations.

Avoid using blob storage like S3 as primary data store. typically need a core database and has pointer to the blobs.

Things should know:
1. incredibly durable.
2. infinitely scalable: no need to discuss in the interview.
3. cost effective, much cheaper than database.
4. build-in security features
5. can upload and download directly from client (?)
6. chunking: allows multipart upload API.
Use S3.
