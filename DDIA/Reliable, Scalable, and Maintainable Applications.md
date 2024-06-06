Data-Intensive: CPU power is not a limiting factor, bigger problems are the amount of data, the complexity of data, and the speed of changing.
Typically components: databases, caches, search indexes, streaming processing, and batch processing. \
# Thinking about Data Systems
Why we unify databases, queries, caches etc as data systems:
1. Many new tools for data storage and processing have emerged.
2. For many applications, a single tool can not longer meet all of its need. Example: application with cache and database usually hide implementation details from clients.
Question to ask myself:
1. how to ensure data correctness and completeness (when things go wrong)
2. how to provide good performance to clients (even part of the system are degraded)
3. how to scale to handle increase in load
4. API design.
This book is about:
1. Reliability: the system should work even in face of adversity
2. Scalablity: as the system grow the system should handle the load.
3. Maintainablity: people work on the system should be able to work on it productively.
# Reliablity
Continue to work correctly, even when things go wrong.\
Fault is different from failure: Fault is one component of the system deviating from spec, failure is the system as a whole stops working. In such fault-toleant system, we can increase the rate of fault by triggering them deliberately: we can ensure that the fault -tolerance machinery is continually exercises and tested.
In general, tolerating faults is better than preventing faults.

## Hardware Faults

   

