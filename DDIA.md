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

   
