**Master Node**
    - runs *Nimbus* daemon
    - distributes code around cluster
    - assigns tasks to machines
    - monitors failures

**Worker Nodes**
    - Runs *Supervisor* daemon
    - listens for work asigned to its machines
    - starts and stops worker processes based on *Nimbus* assignments
    - *Worker Processes* execs subset of topology

**Topology**
    - consists of many *worker processes* across many machines
    - *graph computation* 
        - links between nodes is how data is passed around
        - ` storm jar <name.jar> <topology_main_class> <args>

### Stream 
- unbounded sequence of tuples
- streams are transformed into new streams in distributed and reliable manner
**Spout** - source of stream
**Bolt**
    - consumes any number of streams, processes, possibly emits new streams
    - Runs Functions, filters tuples, stream aggregation, joins, DBs

### Topology
    - network of spouts and bolts
    - submitted to strom cluster
    - edges in graph indicate which bolts are subscribing to which streams
    - emitted tupe sent to all subscribed

### Tuple
    - named list of values
        - field can be any object type
        - out of box -> primitive types, strings, byte arrays
        - implement serializer for other types
    - every node in topology must declare *Output Fields* it emits in tuple

**Parallelism**
    - how many threads should execute component across cluster
    - default is 1

**Shuffle Grouping**
    - tuples randomly distributed from input tasks to bolt tasks


- Can determine source of tuple if subscribed to multiple streams
- cleanup in Bolt not guaranteed to be called

**Local Mode**
    - execs in process
    - simulates worker nodes with threads

** Distributed Mode**
    - submit to master
    - workers go down, reassigns

**Num workers**
    - processes allowed around cluster to execute topology
    - number of threads allocated to component configured in setBolt, setSpout
    - threads are within a worker process
    - i.e. 300 threads across cluster
        - 50 workers => 6 threads / worker across all its components

### Stream Grouping
    - how to send tuples between two copmonents
    - **Shuffe** - sends tuple to random task, evenly distributes
    - **Field** 
        - same <field> always goes to same task
        - group by subset of fields
        
