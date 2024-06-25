This chapter examines techniques for sharing and publishing objects so they can be safely accessed by multiple threads.

Synchronization also has another significant, and subtle, aspect: memory visibility.
# Visibility
In the absence of synchronization, the Java Memory Model permits the compiler to reorder operations and cache values in registers, and permits CPUs to reorder operations and cache values in processor-specific caches.

Nonatomic 64-Bit Operations:
Out-of-thin-air safety: it sees a value that was actually placed there by some thread rather than some random value, 

For long and double variables, the JVM is permitted to treat a 64-bit read or write as two separate 32-bit operations. If the reads and writes occur in different threads, it is therefore possible to read a nonvolatile long and get back the high 32 bits of one value and the low 32 bits of another.

In other words, everything A did in or prior to a synchronized block is visible to B when it executes a synchronized block guarded by the same lock.

Rule requiring all threads to synchronize on the same lock when accessing a shared mutable variable—to guarantee that values written by one thread are made visible to other threads.

Locking is not just about mutual exclusion; it is also about memory visibility. 

The visibility effects of volatile variables extend beyond the value of the volatile variable itself. When thread A writes to a volatile variable and subsequently thread B reads that same variable, the values of all variables that were visible to A prior to writing to the volatile variable become visible to B after reading the volatile variable.

The server JVM performs more optimization than the client JVM, such as hoisting variables out of a loop that are not modified in the loop; code that might appear to work in the development environment (client JVM) can break in the deployment environment (server JVM).

The semantics of volatile are not strong enough to make the increment operation (count++) atomic, unless you can guarantee that the variable is written only from a single thread.

Locking can guarantee both visibility and atomicity; volatile variables can only guarantee visibility.

# Publication and Escape
Publishing an object means making it available to code outside of its current scope. 
