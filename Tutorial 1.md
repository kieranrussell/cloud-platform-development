# Tutorial 1

## Question 1

What are the four workload patterns of application use that may benefit being
deployed to a cloud computing environment?

## Answer 1

- On & off workloads (e.g. batch job)
  - e.g. scientists running modelling software for new drug installed capacity is wasted when not being used, but: users idle while waiting for jobs to finish…
- Growing fast
  - successful services need to grow and scale e.g. new Internet game that catches on
  - deployment and scaling lags stunts market growth at key critical moment
  - need capital for software development or marketing instead of building data centre
- Predictable bursting
  - many services have seasonal trends, either macro (festivals) or micro (sports events) or peak hours
  - peak load can exceed average load by factor 2x-10x
  - but: few users deliberately provision for less than the peak
  - result: server utilization in existing data centres ~5%-20%!!
  - dilemma: waste resources or lose customers!
- Unpredictable bursting
  - Unexpected/unplanned peak in demand e.g. important breaking news events
  - can’t afford to provision for extreme case, but failure to handle it well can destroy business
  - important that service level agreement for such business areas e.g. the press, are previously agreed

## Question 2

Discuss the five main principles of cloud computing.

## Answer 2

### Pooled resources
- commodity computing
  - pay as you go
  - available to anyone
  - exploits economics of volume
  - massive replication of identical hardware units
- multi-tenanted
  - potential security/legal issues
  - shift from capital expenditure to operational
- expenditure
  - lowered barrier for project starts

### Virtualisation
- facilitates consolidation i.e. the need to get maximum
  utilisation of physical cloud servers by hosting multiple
  virtualised servers on each
  - to efficiently use space/power/cooling...
- the large pool of virtualised servers is the primary unit to
  be allocated to consumers as needed
  - applications must be able to exploit this
- sandboxing aids multi-tenanting
- each virtual server becomes a (system) virtual
  machine
- technical details in future lecture

### Elasticity
- dynamic scaling i.e. change how much of
a resource is consumed in response to
how much is needed
  - normally need a base set of resources
    but much more under peak conditions
- scale out rather than scale up
  - we add more identical virtual servers
  - we do not add more hardware to
    underpin a single virtualised server

### Automation

- infrastructure is required:
  - for the user and system admin to create and
    delete virtual servers both manually and
    automatically
    - and to pick how many resources they
      have, where they are located etc.
  - to automatically monitor performance metrics
    (CPU loads, queue lengths...) and scale
    accordingly
  - to scale at set times e.g. a peak time of the
    day

### Metered Billing
- typically have startup and annual contract fees
  - beyond that pay as you go
    - based on CPU, no. virtual server instances, storage
      used, I/O...
  - entry barriers much lower than for traditional projects
    - easier for entrepreneurs as no capital investment
    - cost associativity: 1,000 computers for 1 hour same
      price as 1 computer for 1,000 hours
    - access to huge computing resource
      - universities, R&D companies
  - access to Big Data and its processing
    - Hadoop/Map-reduce
    - IBM’s Watson

## Question 3

Within the context of cloud computing, discuss what is meant by a service level
agreement (SLA).

## Answer 3

- For commercial success, cloud data-centres need to provide customers with strict Quality of Service (QoS) guarantees
  - documented as Service Level Agreements
  - only then can customers be confident in outsourcing their jobs to a cloud infrastructure
  - may differ in application type and workload e.g.
    - transactional applications require response time and throughput guarantees
    - non-interactive batch job concerns performance (i.e. completion times)
    - web applications tend to be highly unpredictable and bursty in nature
  - may be complex to supply an infrastructure that can satisfy these competing goals whilst at the same time using the data-centre hardware efficiently

## Question 4

Discuss some cases in which a cloud solution may not be useful.

## Answer 4

- Control over Server
  - CPU, memory, other specs
- Latency Concerns
  - Slower
- Legislative Issues
  - Access to data
- Geopolitical Concerns
  - Cloud out of UK


## Question 5

Discuss a key issue of distributed systems.

## Answer 5

- Notice that the cloud is implicitly a distributed system.
  - what we have are a set of multiple virtual servers
    - these are going to have to communicate to get the work done
    - and/or access shared data in a safe manner
  - developing distributed applications is hard
  - however many of the problems are well understood and their solutions can be leveraged in the cloud
- However a cloud deployment probably has many more (virtualised) servers than a traditional distributed system.
  - many more points of failure
  - **faults** are likely and have to be managed, include:
    - disk failures
    - network partitioning
    - bugs
    - human factors....
  - on a large scale, *faults will be commonplace*
