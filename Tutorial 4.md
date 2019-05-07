# Tutorial 4

## Question 1

Compare and contrast vertical and horizontal scalability. Which is more applicable to cloud computing?

## Answer 1

- **Horizontal Scalability**:
  - adding multiple logical units of resources and making them work as a single unit. Most clustering solutions, distributed file systems, load balancers aid horizontal scalability. Usually this is achieved using less powerful machines than the single one used in vertical scalability above. Sometimes this is termed **scaling out**.
- **Vertical Scalability**:
  - adding resource within the same logical unit to increase capacity. An example of this would be to add CPUs to an existing server, and more memory and/or expanding storage by adding hard drive on an existing RAID SAN storage. Sometimes this is termed **scaling up**.

**Cloud computing** is a key example of **horizontal** scalability.

## Question 2

Discuss what is meant by the scalability factor of a system.

## Answer 2

- Every component whether processors, servers, storage drives or load balancers have some kind of management/operational overhead.
- When attempting to increase the scale of a system by introducing new components or, increasing the capacity of existing ones, it is important to understand what percentage of the newly configured system is actually usable.
- This measurement is called the **scalability factor** i.e. if only 95% of the expected total new processor power is available when adding a new CPU to the system, then the scalability factor is 0.95.

We can further quantify scalability factor as:

- **linear scalability**: the performance improvement remains
proportional to the upgrade as far as you scale i.e. a scalability
factor of 1.0.
- **sublinear scalability**: a scalability factor below 1.0. In reality some
components will not scale as well as others. In fact the scalability
factor may itself tail off as more resources are progressively added.
  - sub-linear scalability likely to be the reality
  - see slides on Amdahl/USL at end
- **superlinear scalability**: although rare, it is possible to get better than expected ( > 1.0) performance increases e.g. some Hadoop cloud applications
  - however Gunther has shown that this behaviour is exhibited at small scale only with a penalty as the systems gets larger
- **negative scalability**: if the application is not designed for scalability, it is possible that performance can actually get worse as it scales.

## Question 3

A cloud vendor provides several different options for compute intensive workloads by offering a choice of virtual machines differing by number of virtualised CPU cores.
It is found that for an application σ = 0.5. Using Amdahl's law calculate the speedup of the application when attempting to scale from a virtual machine with 1 virtualised core to one with 8 cores and then 32 cores. Compare the answers for a program with σ = 0.05

For the σ = 0.5 scenario, comment on the effect of scaling up to even more cores.

## Answer 3

Amdahl's law: Speed up factor `S = N/(1 + σ (N – 1))`

```
σ = 0.5

N = 8
S = N/(1 + σ (N – 1))
  = 8/(1+0.5*7)
  = 8/4.5
  = 1.78

N = 32
S = N/(1 + σ (N – 1))
  = 32/(1 + 0.5*31)
  = 32/16.5
  = 1.94
```

```
σ = 0.05

N = 8
S = N/(1 + σ (N – 1))
  = 8/(1 + 0.05*7)
  = 8/1.35
  = 5.93

N = 32
S = N/(1 + σ (N – 1))
  = 32/(1 + 0.05*31)
  = 32/2.55
  = 12.55
```

Scaling out to more processors for σ = 0.5;
It can be seen that there is only a marginal increase.

```
σ = 0.5

N = 128
S = N/(1 + σ (N – 1))
  = 128/(1 + 0.5*127)
  = 128/64.5
  = 1.98

```

## Question 4

What is meant by the Universal Scalability Law, and in that context, contention and coherence? When does it reduce to Amdahl’s Law and linear scalability?

## Answer 4

- Will look at how to measure the effectiveness of scaling i.e. to determine a cost/benefit analysis of adding hardware:-
- **Amdahl’s Law**: really a better measure for supercomputers and clusters involved in HPC (high-performance computing).
  - however an interesting insight into the limits of scalability
- **Universal Scalability Law**: a more realistic measure of scalability in distributed systems taking into account overheads of communication and synchronisation.
  - A more realistic extension of Amdahl’s Law for general cloud applications.
    - A new factor is included in equation:-
  - **coherence** (κ): the cost of consistency e.g. the time it takes to synchronise data in different nodes in a system or to introducing locking i.e. the penalty incurred to make resources consistent
  - In practice coefficient values for contention and coherence are determined experimentally from controlled experiments and then used to determine how well the system will scale with more hardware added.
    - allows determination of optimal optimisation and system bottlenecks

The Universal scalability law is described as:

```
𝑺 = 𝑵/(𝟏 + 𝝈(𝑵 − 𝟏) + 𝜿𝑵(𝑵 − 𝟏))
```

It reduces to Amdahl's Law for `𝜿 = 0`

It reduces to model linear scalability, for `𝝈 = 0` and `𝜿 = 0`


## Question 5

For a cloud-based web service application, its contention and coherence
coefficients are measured to be 0.25 and 0.01 respectively.

Using the Universal Scalability Law determine whether it is worthwhile to scale from a system with 2 cores to 4, 8 or 16 cores (or even beyond this). Comment on what would happen if the coherence value could, if possible, be reduced towards zero? Assume the server nodes have one core each.

## Answer 5

```
𝑺 = 𝑵/(𝟏 + 𝝈(𝑵 − 𝟏) + 𝜿𝑵(𝑵 − 𝟏))

with σ = 0.25 and κ = 0.01

N = 2
S = N/(1 + σ (N – 1) + 𝜿N(N - 1))
  = 2/(1+0.25*1 + 0.01*2*1)
  = 2/(1.25+0.02)
  = 1.57

N = 4
S = N/(1 + σ (N – 1) + 𝜿N(N - 1))
  = 4/(1+0.25*3 + 0.01*4*3)
  = 4/(1.75+0.12)
  = 2.13

N = 8
S = N/(1 + σ (N – 1) + 𝜿N(N - 1))
  = 8/(1+0.25*7 + 0.01*8*7)
  = 8/(2.75+0.56)
  = 2.41

N = 16
S = N/(1 + σ (N – 1) + 𝜿N(N - 1))
  = 16/(1+0.25*15 + 0.01*16*15)
  = 16/(4.75+2.4)
  = 2.23

N = 32
S = N/(1 + σ (N – 1) + 𝜿N(N - 1))
  = 32/(1+0.25*31 + 0.01*32*31)
  = 32/(8.75 + 9.92)
  = 1.71
```

In fact performance peaks about N = 8 so there is no point in increasing the cores (and in this scenario servers) unless the architecture and operation of the application is reviewed to revise the values of σ and κ.

Reducing κ towards 0 (if possible) would also only reduce the issue to Amdahl’s Law and it can be argued that with this data beyond 32 cores it is the law of diminishing returns for increasingly expensive processor outlay.

```
S = N/(1 + σ (N – 1))

N = 2
S = N/(1 + σ (N – 1))
  = 2/(1+0.25*1)
  = 2/1.25
  = 1.6

N = 8
S = N/(1 + σ (N – 1))
  = 8/(1+0.25*7)
  = 8/2.75
  = 2.90

N = 32
S = N/(1 + σ (N – 1))
  = 32/(1+0.25*31)
  = 32/8.75
  = 3.65

N = 128
S = N/(1 + σ (N – 1))
  = 128/(1+0.25*127)
  = 128/32.75
  = 3.90
```

## Question 6

Compare and contrast cloud storage types and their implementation i.e. blobs, queues and non-relational tables. Comment on their scalability.

## Answer 6

- **BLOB** (Binary Large Object)
  - Storage of an unstructured binary object in a single partition.
  - Contents are opaque to the storage service.
  - Blob has settable metadata.
    - e.g. a name allowing one blob to be distinguished from another
  - Stored in blocks (optional access to this level for effective random access).
  - Can map file system to a blob for legacy applications.
  - Example of content: videos, songs, pictures, newspaper articles.
  - Azure Blobs or AWS Simple Storage Service (S3)
  - Containers (buckets in S3) facilitate an administrative breakdown of the blob space.
    - provides a namespace mechanism
  - The key value identifying the partition is the container name + blob name.
    - each blob has its own partition
    - blobs can be distributed across many servers


- **Queues**
  - A queue is a classic first-in, first-out data storage structure.
    - Primarily used for passing data from one computing job to another in a loosely-coupled fashion.
      - typically between role instances
  - Generally not used for long-term storage.
  - Scale really well.
    - there are limits on the rate of message processing
    - if the rate proves insufficient can be scaled by creating multiple queues
  - The key for a message is the queue name.
    - all messages in the same queue are grouped into a single partition and are served by a single server
    - different queues may be processed by different servers i.e. to balance the load for multiple queues
  - Azure Queues or AWS Simple Queue Service (SQS).
    - offer similar feature sets with minor differences in QoS

- **Tables**
  - Semi-structured data without schemas.
    - Basically collections of name-value pairs.
  - You can use them in any convenient way, like a property bag.
    - each row/entity can have a different set of properties
  - However probably most useful in the classic row-column case.
  - Primary difference from relational tables is the absence of relations and thus referential integrity checks e.g. no joins are available.
    - scale much better than relational tables
    - it is possible to work round the lack of relations but the functionality has to be “hand crafted”
  -  The programmer has control over the entity/row partitioning i.e. what partition has what entities.
    - at least logically to ensure that a subset of entities can be kept together
    - need to balance the scalability benefits of spreading entities across multiple partitions with the data access advantages of grouping entities in a single partition
      - apart – better load balancing and scalability
      - together – atomic batch operations possible e.g. limited transactional operations across multiple entities
  - DynamoDB is an AWS service offering similar functionality with some enhanced features but runs as a service

## Question 7

Discuss the issues and approaches to scaling large relational databases in the cloud.

## Answer 7

### Relation Databases

- Q: What’s the problem?
- A: Relational tables are difficult to scale out well.
  - by definition relational databases contain necessary and complex relations between tables
- The reason these do not scale well is because of the need to enforce
**referential integrity**.
  - this check is undertaken by the dbms itself
  - the dbms needs to be able to communicate between all tables involved at the same time
- This ability degrades as the number of tables and relations increases
and with the number of rows in each table.
  - may be able to scale up to a certain degree, but scaling out difficult
- Important observation: most data services are read-dominant.
  - database replication works well for them


Some of the scalability techniques used predate the cloud and have been leveraged.

- Functional Partitioning
  - distribute complete tables
  - limiting the scope of UDI queries i.e. UPDATE, DELETE, INSERT
- Vertical Partitioning
  - split tables by columns
  - denormalisation
- Horizontal Partitioning
  - sharding
  - split tables by rows
- A relational database designed for the cloud (e.g. Azure SQL) will offer a mix of these techniques but their actual use is up to the database designer and is application dependent.
  - none of this can be applied automatically…
