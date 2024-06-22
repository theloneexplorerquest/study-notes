Thread-safe code is about managing state, in particular to shared, mutable state.

Making an object thread-safe requires using synchronization to coordinate access to its mutable state.

A program that omits sync might appear to work but will fail at moment.

If multiple thread access the same mutable state:
1. Don't share state at all
2. Make the state immutable
3. Use synchronization whenever possible.

It is eacher to design class to be thread-safe than retrofit it.

Techniques: encapsulation, immutability, and clear specification of invariants.

# What is Thread Safety
A class is thread-safe if it behaves correctly when accessed from multiple threads, regardless of the scheduling or interleaving of the execution of those threads by the runtime environment, and with no additional synchronization or other coordination on the part of the calling code.

Stateless objects are always thread-safe.

# Automicity
++count is not a single action, it is not atomic, this is an example of read-modify-write (lost update).

Automicity is crucial for generate sequences or unique object identifiers.

A race condition occurs when the correctness of a computation depends on the relative timing or interleaving of multiple threads by the runtime.

Characterizes most race conditions—using a potentially stale observation to make a decision or perform a computation. 

check-then-act: you observe something to be true (file X doesn't exist) and then take action based on that observation (create X); but in fact the observation could have become invalid between the time you observed it and the time you acted on it
