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
Many to one relationships: many people live in one particular region etc. In relational databse, we just refer rows in other tables by ID since join is easy. In document model, since join is not required for one-to-many tree, support for join is weak (so we have to do join in application code). This is problematic, data becomes more interconnected as features are added to applications.
Many-to-one and Many-to-Many Relationships require joins which is not supported in document model.

## Are document databases repeating history?
