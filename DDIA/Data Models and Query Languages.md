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
between objects and database model: impedance mismatch.
