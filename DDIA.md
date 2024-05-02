# Part III Derived Data
motivation: integrate multiple different data systems, different model and different access patterns.
data system:
1. system of record: source of truth. from user input
2. derived data systems: transform or processing existing data. Reduntant, denomalized, for good performance.
   
Database don't distinguish: it is just a tool.
## Chapter 10 Batch Processing
First two part is about online system.
Three types of systems:
1. Online Systems: send back as soon as possible, availablity
2. Offline Systems - batching processing: large input, job takes a while, run on schedule, throughput.
3. Streaming Systems - near real-time: somehow in between, consume input and produce outputs. Operate on event shortly after happen.
### Batch Processing with Unix Tools
1. Chain of commands vs custom program (Ruby script): custom program is less concise, however more readable
2. sorting vs in-memory aggregation: Ruby script keeps an in-memory hash table. If the working set is large, Unix is can efficiently use disk (SST, LSM).
   
Unix Philosophy: each program do one thing well, expect the output of every program to become the input to another.\
In Unix, the interface is a file, it is still something remarkable.\
stdin and stdout: shell user to wire up the input and output whatever they want (loose coupling, one does not care about another).\
The input is immutable.\
Drawback: program that need multiple input or output.\
However it only run on a single machine!\
### MapReduce and Distributed Filesystems
