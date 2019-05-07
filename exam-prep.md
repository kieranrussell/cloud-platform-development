# Exam Revision

## Question 1 (a)

The five main principles of cloud computing can be defined as:
 - Pooled Resources
 - Virtualisation
 - Elasticity
 - Automation
 - Metered Billing

Briefly discuss what is meant by each of these principles.

*(10 marks)*

## Answer 1 (a)

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

## Question 1 (b)

Discuss the NIST IaaS and PaaS cloud service delivery models.

*(6 marks)*

## Answer 1 (b)

There are 3 service delivery models:
- Infrastructure as a Service (IaaS)
- Platform as a Service (PaaS)
- Software as a Service (SaaS)

### IaaS

- The cloud vendor provides a rentable infrastructure to
  support virtualization of OSs and network.
  - vendor outsources the equipment used to support operations,
    including storage, hardware, servers and networking components
  - the service provider owns the equipment and is responsible for
    housing, running and maintaining it
  - i.e. it provides a mechanism to provision physical or virtual machines
- Typically the customer provides virtual machine images and
  installs any software dependencies thereon themselves.
  - cloud APIs, necessary software APIs, databases...
  - this is more work than PaaS but offers more flexibility can provide
    own OS images (or again pick from library) and installed software
  - the customer will administer the VM operating system
  - sometimes an easier transition to the cloud for complex legacy
    systems

- Customer has full control to virtualise a network set-up.
  - also to configure firewalls, load balancers
- Amazon EC2, Microsoft Azure Virtual Machines,
  Rackspace, Google Compute Engine, IBM SoftLayer

### PaaS

- The cloud vendor provides and manages a set of
  preconfigured VMs which the customer selects from.
- Typically the preconfigured VM OS images will be
  mainstream offerings with pre-installed software to run
  cloud applications.
  - cloud APIs e.g. to access cloud storage, databases, service bus,
    performance metrics, security, application services...
  - optionally also a web server e.g. IIS or Apache
  - developed software is uploaded to VM from local development tools
  - customer has initial choice of OS image, VM location, scalability
    parameters, security but this is high level and at start
  - the customer will not administer the VM operating system nor deal
    with hardware
  - much simpler and faster to setup than IaaS
    - in the order of minutes...

- Network set-up virtualised but typically abstracted and customer has no control.
  - better control is gradually being introduced (e.g. VPN control)
- AWS Elastic BeanStalk, Microsoft Azure Cloud
  Services and Web Apps, IBM BlueMix, Google App
  Engine...
- The practical work in this module is using PaaS i.e.
  Microsoft Web Apps in a public cloud.

### SaaS

- The cloud vendor provides packaged software applications
  running in the cloud aimed at end-users.
  - customer has no visibility of software, OS nor hardware
    underneath
- Especially productivity and collaboration applications.
  - Google Apps, Office 365, Visual Studio Online, cloudbased file sharing....
- Available on an on-demand basis.
- Usually accessed through a web browser.

## Question 1 (c)

A cloud infrastructure hosts a stateless web server system, replicated
on four virtual servers (each virtual server is underpinned by a
separate real (physical) server). The cloud infrastructure **service level
agreement** (SLA) indicates that each virtual server has a mean time
between failures of 10 days; a failure on average takes 20 hours to
fix. Calculate the aggregate **availability** of the cloud-based web
server system.

*(4 marks)*


## Answer 1 (c)

```
MTTR = 20 hours
MTBF = 10 days

ρ = MTTR /(MTBF + MTTR)
  = 20/((10 * 24) + 20)
  = 20/(240 + 20)
  = 20/260
  = 0.077

= 1 – ρ^n
= 1 - 0.077^4
= 1 - 0.00003515
= 0.999964
= 99.99%
```

## Question 2 (a)

Discuss how **Service Oriented Architecture (SOA)** principles can
be applied to cloud application design.

*(8 marks)*

## Answer 2 (a)

- A service-oriented architecture is essentially a collection
of services. These services communicate with each other.
The communication can involve either simple data passing
or it could involve two or more services coordinating some
activity.
  - a higher level than classes – here we are talking about the whole self
    – contained service (application) which is a black box, can be used by
      consumers from a different ownership domain, is loosely coupled
      from the user and can be discovered at run-time.
- SOA is a principle adopted in modern enterprise distributed
  system design and is also applicable to cloud computing.
  - we will also discuss microservices


## Question 2 (b)

Compare and contrast four approaches that may be used to
implement **load balancing** in the cloud.

*(8 marks)*

## Answer 2 (b)

### Load balancing

share the load fairly between running service instances thereby maximising throughput

- Various approaches are used based on:-
  - performance – directs client to the best load balanced node based on its latency or current connection count
  - failover – directs client to the primary node unless the primary node is down and then redirects to a backup node.
  - round robin – directs client to the nodes in a cyclic fashion hash function – uses a hash of the protocol characteristics (e.g.
  client TCP connection) to determine the node
- The load balancer includes some functionality to probe the
  nodes to discover the appropriate metric.
  - TCP/HTTP/DNS latency
  - liveness
- Azure and AWS incorporate load balancers
  - more sophisticated functionality available from 3rd parties
    - Kemp, Barracuda, Citrix…

### Load balancing in azure

- A few options but the basic one is a Transport Layer load balancer based on hash based distribution.
  - Internet facing
- Uses is a 5 field (source IP, source port, destination IP, destination port, protocol type) hash to map traffic to available servers.
  - all traffic from same client in a TCP connection goes to same server
    – sticky sessions
- but for a new connection with same values goes to another
  - so in a sense incorporates some round-robin features

## Question 2 (c)

For a given service running at a server, its **contention** is 0.25. Using
**Amdahl’s Law** calculate the speedup of the service when
attempting to scale up progressively from 1 core to 8 cores and then
to 32 cores. Comment on the effect of introducing more cores.

*(4 marks)*

## Answer 2 (c)

```
S = N/(1 + σ (N – 1))

σ = 0.25

N = 1
S = N/(1 + σ (N – 1))
  = 1/(1 + 0.25 * (1 - 1))
  = 1/1
  = 1

N = 8
S = N/(1 + σ (N – 1))
  = 8/(1 + 0.25 * (8 - 1))
  = 8/(1 + 0.25 * 7)
  = 8/(1 + 1.75)
  = 8/2.75
  = 2.909

N = 32
S = N/(1 + σ (N – 1))
  = 32/(1 + 0.25 * (32 - 1))
  = 32/(1 + 0.25 * 31)
  = 32/(1 + 7.75)
  = 32/8.75
  = 3.657

N = 64
S = N/(1 + σ (N – 1))
  = 64/(1 + 0.25 * (64 - 1))
  = 64/(1 + 0.25 * 63)
  = 64/(1 + 15.75)
  = 64/16.75
  = 3.820

N = 128
S = N/(1 + σ (N – 1))
  = 128/(1 + 0.25 * (128 - 1))
  = 128/(1 + 0.25 * 127)
  = 128/(1 + 31.75)
  = 128/32.75
  = 3.908

```

By scaling out to more processors for `σ = 0.25` it can be seen that there is only a marginal increase.

## Question 3 (a)

Compare and contrast the following approaches that can be used to sacle large **relational databases** in the cloud.

- functional paritioning
- vertical partitioning
- horizontal partitioning

*(12 marks)*

## Answer 3 (a)

- **Functional Partitioning**
  - distribute complete tables
  - limiting the scope of UDI queries i.e. UPDATE, DELETE, INSERT
  - We must send less UDIs to each server.
    - each server contains a subset of all tables
      - note tables are intact here
    - limits the scope of UDI queries
  - Updates to T1 must be addressed to only 2 servers.
  - We must place tables according to query templates.
    - we cannot execute a query that joins T1 and T2. . .


- **Vertical Partitioning**
  - split tables by columns
  - denormalisation
  - Reduces the I/O and performance costs associated with
    fetching the items that are accessed most frequently.
    - column-level oriented
    - schema has to be reorganised
    - columns with common access behaviours can be held together and   optimised separately – e.g. speeded up with the introduction of a cache
    - different collections of columns can be secured separately
  - Denormalisation is a variant of vertical partitioning.
  - Intentionally duplicate columns across multiple tables.
    - denormalise data into separate services
    - replicate read-only columns across multiple tables and data services


- **Horizontal Partitioning**
  - sharding
  - split tables by rows
  - Also commonly called sharding.
  - Notice all shards have the same schema.
  - Most queries are indexed by primary key.
  - Common approach is to hash table records by their primary key.
    - other mathematical functions are used too e.g. modulo
    - split hash space evenly between shards (i.e. servers)
    - can be difficult to later rebalance the data distribution...
    - as the hashed key value is random this ensures balanced distribution of rows between shards and can ensure balanced workload
      - avoids “hot partitions”
  - However main requirement is to balance requests to shards and not to ensure they should contain same amount of data.
    - important to ensure that a single shard does not exceed the scale limits (in terms of capacity and processing resources) of the data store being used to host that shard

A relational database designed for the cloud (e.g. Azure SQL) will offer a mix of these techniques but their actual use is up to the database designer and is application dependent.
  - none of this can be applied automatically…

## Question 4 (a)

Discuss **OpenID Connect**. Your answer should clearly explain OpenID's relationship to OAuth 2.0. How is OpenID used in the cloud?

*(8 marks)*

## Answer 4 (a)

- OAuth
  - optional on HTTPS but increasingly everywhere especially in the cloud.
  - *authorisation* i.e. what are you alowed to do to what resources.
- OpenID
  - optional on OAuth and provides distributed, federated third-party identity guarantee service.
  - *authentication* of user to the server

## Question 4 (b)

Compare and contrast **transport security** and **message security** as used in web services.

*(6 marks)*

## Answer 4 (b)

- **Message Security** - SOAP
  - **Pros**
    - Different parts of a message can be secured in different ways.
    - Asymmetric: different security mechanisms can be applied to request and response
    - Self-protecting messages (Transport independent)
  - **Cons**
    - Secure the message – don’t need underlying protocol to be secure
    - Securing XML is complicated and an overhead

- **Transport Security** - TLS/SSL & HTTPS - REST
  - **Pros**
    - Widely available, mature technologies (SSL, TLS, HTTPS).
    - Understood by most system administrators
  - **Cons**
    - Point to point: The complete message is in clear after each hop
    - Symmetric: Request and response messages must use same security properties
    - Transport specific

## Question 4 (c)

Demonstrate using the **RSA algorithm** the encryption and decryption in the integer 2. Show using the standard **totient** algorithm
`(t(n) = (p - 1) * (q - 1))`
how the public and private key values are calculated. Assume p = 3 and q = 11.

*(8 marks)*

## Answer 4 (c)
