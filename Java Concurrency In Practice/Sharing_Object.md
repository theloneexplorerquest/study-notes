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

# Thread Confinement

# Immutablity
An object is immutable:
1. Its state cannot be modified after contruction.
2. All fields are final.
3. It is properly constructed (this reference does not escape during construction)
Make all fields private unless they need greater visibility, it is a good practice to make all fields final unless they need to be mutable.

# Safe Publication
Simply storing a reference to an object into a public field, is not enough to publish that object safely.

the Object constructor first writes the default values to all fields before subclass constructors run. It is therefore possible to see the default value for a field as a stale value.

Immutable objects can be safely accessed even when synchronization is not used to publish the object reference.If final fields refer to mutable objects, synchronization is still required to access the state of the objects they refer to.

Place into Hashtable, synchronizedMap, or ConcurrentMap, Vector, CopyOnWriteArrayList, CopyOnWrite-ArraySet, synchronizedList, BlockingQueue or a ConcurrentLinkedQueue safely publishes it to any thread that retrives it from the map.

Static initializers are executed by the JVM at class initialization time; this mechanism is guaranteed to safely publish any objects initialized in this way.

If an object may be modified after construction, safe publication ensures only the visibility of the as-published state. Synchronization must be used not only to publish a mutable object, but also every time the object is accessed to ensure visibility of subsequent modifications. 

The most useful policies for using and sharing objects in a concurrent program are:
1. Thread-confined.
2. Shared read-only. Shared read-only objects include immutable and effectively immutable objects.
3. Shared thread-safe. A thread-safe object performs synchronization internally, so multiple threads can freely access it through its public interface without further synchronization.
4. Guarded. A guarded object can be accessed only with a specific lock held. Guarded objects include those that are encapsulated within other thread-safe objects and published objects that are known to be guarded by a specific lock.
