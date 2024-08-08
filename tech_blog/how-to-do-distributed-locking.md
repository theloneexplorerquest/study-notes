https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html
Why we need distributed lock:
1. Efficiency: Taking a lock saves you from unnecessarily doing the same work twice
2. Correctness: Taking a lock prevents concurrent processes from stepping on each others’ toes and messing up the state of your system.
If merely for efficiency purpose, it is unnecessary to incur the cost and complexity of redlock.
