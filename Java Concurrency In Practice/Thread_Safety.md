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

check-then-act: you observe something to be true (file X doesn't exist) and then take action based on that observation (create X); but in fact the observation could have become invalid between the time you observed it and the time you acted on it.

# Compound Actions
Operations A and B are atomic with respect to each other if, from the perspective of a thread executing A, when another thread executes B, either all of B has executed or none of it has.

# Locking
 thread safety requires that invariants be preserved regardless of timing or interleaving of operations in multiple threads.

 Java provides a built-in locking mechanism for enforcing atomicity: the synchronized block. Every Java object can implicitly act as a lock for purposes of synchronization.

Reentrancy means that locks are acquired on a per-thread rather than per-invocation basis. Reentrancy is implemented by associating with each lock an acquisition count and an owning thread.

# Guarding State with Locks
Compound actions on shared state, such as incrementing a hit counter (read-modify-write) or lazy initialization (check-then-act), must be made atomic to avoid race conditions.

Holding a lock for the entire duration of a compound action can make that compound action atomic. 

if synchronization is used to coordinate access to a variable, it is needed everywhere that variable is accessed.

when using locks to coordinate access to a variable, the same lock must be used wherever that variable is accessed.
