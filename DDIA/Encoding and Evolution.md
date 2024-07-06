We should aim to build system that make it easy to change.

Code change in large application:
1. server side: rolling upgrade.
2. client side: at the mercy of user.

Therefore we need to maintain compatibility (backward and forward). Forward compatibility require old code to ignore additions from new code.
# Formats for Encoding Data
data in two representations:
1. In memory, data is kept in object, struct, list etc.
2. send over network, encode it as sequence of bytes.

We need to encode and decode.

## Language-Spcific Formats
Java has java.io.serializable, Python has pickle. These encoding libray are convenient since minimal additional code is required. Problems:
1. the encoding is tied to particular language.
2. To restore in the same object types, the decoding process need to instantiate arbitrary classes. Security issue as attacker can get application to run arbitrary code.
3. versioning a lot, they often neglect the forward and backward.

## JSON, XML, Binary Variants
1. XML: verbose and complicated
2. JSON: build-in support in web broswer and simplicity.
Problems:
1. a lot of ambiguity: XML and CSV cannot distinguish between number and string. Json don't distinguish integers and float-point numbers. There is problem with large number as well.
2. JSON and XML don't support for binary string, waste data size.
3. CSV does not have any schema.

### Binary encoding
For data that is used only internally, there is less pressure to use a common denominator encoding format. An example is JSON MessagePack.
### Thrift and Protocol Buffers
Both require a schema for any data that is encoded. Thrift and Protocol come with code generation tool that takes a shema def and produce class that implement the schema. 

Thrift binary protocol has no field names, however they have tags, shorter. Compact protocol pack field type and tag number into single bytes and variable-length integers. 

Protocal Buffer is very similar to Compact protocol pack.
### field tags and schema evolution
Field tags are critical to the meaning of encoded data: field name can be changed, field tag cannot be changed.

Add a new field:
1. forward compatibility: you can add you field to schema as long as they have new tag number: if the old code does not reco the tag, it simply ignore that field.
2. backward compatibility: new code can always read old data. If you add new field, they cannot be be required: as new code read data written by old code will fail: the old code will not have written the field.

Removing a field is the same but reversed, so we can only remove an optional field, and never use the same tag number.

### Datatype and schema evolution
## Avro
There is no tag numbers in the schema, there is nothing to identify fields or their datatypes. To parse the binary data, go through the field in order and use the schema to tell you the datatypes in each field. So the data can be decoded if the read and write schema exact same schema. 

Avro writer schema and reader schema do not have to be the same, Arvo libary resolve the differences be looking at writer's chema and reader's schema and translate.

1. reading a field in writer schema not in reader, ignored.
2. reader expect some field writer do not have, default value.
### Schema evolution rules
To maintain compatibility, you may only add or remove a field that has a default value.
How does the reader know the writer's schema? 
1. large file with lots of records: include writer's schema onces.
2. different records: include a version number of schema and records.
3. Sending records over network: negotiate the schema on connection setup.

### Dynamically generated schemas
Avro is friendlier to dynamically generated schemas. If the database schema changed, you can just generate a new Avro schema and export data in the new schema. 

However, if you were using Thrift or Protocol, the field tags would likely have to be assigned by hand.

### code generation and dynamically typed languages:
Thrift and Protocal repy on code gen, this is useful for statically typed languages because it allows efficient in-memory structures to be used. In dynamically typed languages, there is no much point in generting code.

## The merits of Schemas
Textual data formats are widespread, biary encoding is good:
1. more compact
2. The schema is a value form of documentation
3. keeping a database of schema allows check forward and backward compatibility of schema changes.

Allows smae kind of flexiblity as schemaless/schema-on-read JSON database, while providing better guarantee about data and better tooling.

# Model of Dataflow
## Dataflow Through Database
Think of storing something in the db as sending a message to your future self. So backward compatiblity is required.

A value in the database may be written by a newer version of the code and subsequently read by old version of code. So forward compatiblity is often required for databases.

A snag: add a field to schma and the newer code write a value for that new field to the database. Older code read the record update and write back: the desirable behavior is usually for the old code to keep the new field intact.

When an older version of the application updates data previous written by a new version of application, data may be lost if you are not careful.

## different values written at different times
data outlive code: when deploying new code, the old version is replaced within minutes. For data, a five year old data will be there with new data. 

When schema change, an old row is read, the database fills in nulls for any columns that are missing from the encoded data.

## Dataflow Through Service: REST and RPC
client and server: the server expose API over the network, and the client can connect to the servers to make request to that API.

a server can itself be a client to another service, it is used to decompose a large application into smaller services by functionality. This microservice.

different from database: service expose an application-specific API that only allow inputs and outputs that are predetermined by the business logic.

### Web services
When http is used as the underlying protiocal for talking to service:
1. client running on user's devices
2. one service making request to another service.

REST is not a protocol, but a design philosophy that builds upon HTTP.

The API of SOAP is described using XML langugae called WSDL, is not designed to be human-readable. 

### The problem with rpc
RPC try to make remote network service look the same as calling a function or method in your programming language, within the same process. Issues:
1. a local function is predictable, network request is not.
2. a local function return result or throw exception or never return. Network request may return without result for timeout
3. If you retry a failed network request, the request could actually get through and only the response get lost. retry will cause action perform multiple times (unless idempotence).
4. each time a call to local function take same time. network request is slower than functional call.
5. local function can pass it as refereence in local memory, network call need to be encoded.
6. the client and server can be implemented in different language.

REST is good as it does not hide the fact that it is a RPC
### Current directions for RPC
1. new gen of RPC is more explicit about it is a remote request.
2. some of these framworks also provide service discovery.
3. RPC with binary encoding can achive better performance.

### Data encoding and evolution for RPC
Server update first, and then client. Thus, only backward compatiblity is required.

Service compatibility is made harder by the fact that RPC is often used for communication across organizational boundaries, so the provider of a service often has no control over its client and cannot force update. If a compatibility-breaking changes is reqiured, we need to maintining multiple versions.

## Message passing Dataflow
async message passing. advantages:
1. act as buffer
2. automatically redilvery messages
3. avoid sender know to ip and port number
4. allow message to be sent to serveral recipents.
5. decoupling.
However, it is often one way, a sender does not expect to receive a reply.
### message brokers
message brokers don't enforce any data model.

### Distributed actor frameworks

# Summary
