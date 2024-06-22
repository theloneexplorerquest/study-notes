Thread-safe code is about managing state, in particular to shared, mutable state.

Making an object thread-safe requires using synchronization to coordinate access to its mutable state.

A program that omits sync might appear to work but will fail at moment
