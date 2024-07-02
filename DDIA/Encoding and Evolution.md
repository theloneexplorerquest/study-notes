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

