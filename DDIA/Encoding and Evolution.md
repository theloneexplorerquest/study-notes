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



# Model of Dataflow
