Applications are built by layering on top of another. How it is represented in terms of the next-lower layer?
1. model real world in terms of data structures and APIs.
2. When you want to store those data structure, use general-purpose data model: JSON, XML, tables, relational database, or a graph model.
3. Database engineer decided on a way of represent JSON/XML/relational/graph in terms of memory.
4. Hardware engineers decide how to represent bytes in terms of electrical currents.
Each layer hides the complexity of the layers below it by providing a clean model.

# Relational Model Versus Document Model
The root of relation databases lie in business data processing: transaction processing and batch processing. It generalize very well to broad variety of use cases.
The goal of relational database is to hide the implementation behind a cleaner interface.

## the birth of NoSQL
Not-only SQL: 
1. greater scalaiblity (large datasets, high write throughtput),
2. free and open source
3. specialized query operations that are not well supported SQL
4. restrictiveness of relational schema, need for more dynamic and expressive data model

## The Object-Relational Mismatch
Most application development today is done in object oriented programming. If data us stored in relational table, an awkward translation is required
between objects and database model: impedance mismatch.\
In the resume example, there is one to many relationship, can be represent as:
1. SQL, put positions, education and contact information in seperate tables.
2. Later version of SQL add support for structured datatypes and XML, multi-valued data can be stored within a single row.
3. Encode jobs, educations and contact info as JSON or XML, store it on text column, let applicatio0n interpret its structure and content (cannot use database two query).\

Locality: fetch profile in relational example, you need to perform multiple queries or join. In JSON, all information is in one place so one query is sufficent.

## Many-to-one and Many-to-Many Relationships
Database normalization: an example in the resume, store IDs not plain text:
1. consistent style and spelling
2. avoid ambiguity
3. Ease of update
4. Localisation support
5. Better seartch
Anything that is meaningful human may change: write overhead, risks inconsistency, duplications.\

Many to one relationships: many people live in one particular region etc. In relational databse, we just refer rows in other tables by ID since join is easy. In document model, since join is not required for one-to-many tree, support for join is weak (so we have to do join in application code). 

This is problematic, data becomes more interconnected as features are added to applications.
Many-to-one and Many-to-Many Relationships require joins which is not supported in document model.

## Are document databases repeating history?
To solve the limitation of hierarchical model:
1. The network model: a record could have multiple parents. The code for querying and updating the database is complicated and inflexible.
2. The relational model: a collection of rows, no nested structure. no comlpicated access paths. The query optimizer automatically decide which parts of the query to execute in which order and which indexes to use (not application devloper).  Query optimiser are only build once.

In relational model and document model, when represent many-to-one and many-to-many relationship, they are the same: the related item is referenced by a nuique identifier (foreign key in relational model, document reference in document model)

## Relational VS Document Databse Today
The main argument in favor of the document data model are schema flexiblity, locality, closer to data structures used by the applications.\
Relational model provides better supports for join, and many to one and many-to-many relationships.\
### Which data model leads to simpler application code?
If data in the application have document like structure, document model is better. Shredding leads to cumbersome schemas and unnecessary compplicated application code.\
If your application is many-to-many or many-to-one, we can reduce join by denormalising, however application code need to keep the donormalised data consistent. Join can be emulated in the application code, complexity shift and slow.
Not possible to say which data model is better.

### Schema flexiblity
Document database does not enforce any schema on the data, client have no guarantees as to what fields to documents may contains. It is called schema-on-read: the structure of the data is implicit and only intepreted when data is read.\ 
Example: when an application want to change the format of its data:
1. In document database, write new document with new fields and have the code to read old and new cases.
2. In relational model, do a schema change which slowing and requires downtime.

If the data is heterogeneous schema on read is advantages:
1. there are many different types of object.
2. The structure of data is determined by external systems which you have no control or may change overtime.

### Data locality for queries
Document model have performance advantages for storage locality, which only applies if you need a large parts of the document at the same time. If only a small part is required, we still need to load all which can be waste. When update, the entire document needs to be rewritten. 

# Query Languages for Data
1. Imperative language: tells the computer to perform certain operations in a certain order.
2. Declartive query language: just specify the pattern of the data you want, it is up to the database system's query optimizer to decide which index and which join method. Advantages: more concise and easier to work with, hide the implementation details, lead to parallel execution.
## Declarative Queries on the Web
The advantage of declarative lanugageare are not limited to just database. In a web browser declarative CSS is much better than manipulating style in JavaScript.

## MapReduce Querying

# Graph-Like Data Models
Document model: one-to-many or no relationships between records. \
A graph consist of two kinds of objects: vertices and edges. Examples are social graphs, the web graph and road or rail networks.
Graph can represent the same things or different types of objects in a single database.

## Property Graph
A graph store consist of two relational tables, one for vertices and one for edges. 
Important aspects:
1. Any vertex can connect to other vertex with edge
2. give any vertex, you can efficiently tranverse the graph.
Graph are good for evolvability: new features can be easily added.
## The Cypher Query Language
Cypher is a declarative query language for property graphs, you don't need to specify execution details.

## Graph Queries in SQL
Graph data can be represented in a relational database, we can query with SQL as well, however the join is not fixed. We need to use recusive common table expressions (clumsy syntax)

## Triple-Store and SPARQL
