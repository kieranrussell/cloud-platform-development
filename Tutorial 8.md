# Tutorial 8

# Consistency

## Question 1

What is meant by the CAP theorem? How does it affect cloud system design?

## Answer 1

- Brewer 2000
- What goals might you want from a distributed system?
  - **Consistency**: by this it means strong consistency
  - **Availability**: a guarantee that every request receives a response about whether it was successful or failed
  - **Partition-tolerance**: the system continues to operate despite arbitrary message loss (nodes or network faults or outage)
- Brewer proposed that in any distributed system we can satisfy any *two* of these guarantees at the same time, *but not all three*


- *Consistency*: strong consistency is the big thing about transactions.
- *Availability*: users need a response so dbms guarantee good availability.
- so *partition-tolerance* has to be dropped...
  - but if in 2PC you can't connect to a node after acquiring a lock then the resource remains locked and the system is not available...
  - therefore distributed transaction implementations are not a class of applications which exhibit partition-tolerance
  - transaction processing systems are “CA systems”
- This is OK in a lan-based distributed system where you can sometimes drop the partition tolerance requirement by using redundant networks.
  - but it won’t work scaled out in the cloud...


- **Availability**: with web applications the end user will not tolerate response times above a certain level.
  - prioritise availability over consistency
  - AWS say: “even the slightest outage has significant financial consequences and impacts customer trust”
- Partition-tolerance: at scale faults will occur all the time .
  - all cloud applications should be architected to tolerate partitions
- so consistency has to give...
  - the cloud is (largely) an “AP system”
- Cloud based systems usually use weaker eventual consistency.
- However you should note that both consistency and availability are more continuous quantities than suggested on the first “CAP Theorem” slide.
  - many real-world use cases simply do not require strong consistency
  - availability is not all or nothing - has to be traded off against the appropriate level of consistency guarantees needed by a particular application


- Provide a consistent view of distributed data at the cost of blocking access to that data while any inconsistencies are resolved. This may take an indeterminate time, expecially in systems that exhibit a high degree of latency or if a network failure [fault] causes loss of connectivity to one or more partitions.

  or

- Provide immediate access to the data at the rist of it being inconsistent across sites. Traditinoal database management systems focus on providing strong consistency, whereas cloud-based solutions that utilise partitioned data stores are typically motivated by ensuring higher availability, and are therefore more oriented towards eventual consistency.

## Question 2

What are the problems associated with large-scale distributed systems. Differentiate between Strong and Weak consistency.

## Answer 2

#### Problems with large-scale distributed systems
- Latency is zero
  - no it isn't - this is the 2nd fallacy of distributed computing.
  - data propagation is limited to a fraction of the speed of light (~30ms accross the Atlantic)
  - not that an architecture that is tolerant of high latency will operate perfectly well with low latency
- Data is geographically distributred.
- Customers demand reasonably consistent performance around the world.
- Latency tolerance can only be acheived by introducing asynchronous interactions.
  - a shift from the familiar synchronous interactions.


- **Strong consistency**
  - after the update completed, any subsequent access will return the same updated value.
- **Weak consistency**
  - it is **not garunteed** that subsequent accesses will return the updated value.
- **Eventual consistency**
  - specific form of weak consistency
  - it is garunteed that if **no new updates** are made to object, **eventually** all accesses will return the last updated value (e.g. propagate updates to replicas in a lazy, asynchronous fasion)


## Question 3

What is meant by BASE and Eventual Consistency?

## Answer 3

- AN alternative to ACID is **BASE**:
  - **Basically Available**: the system is available and can prvide a fast response, but not necessarily all replicas are available at any given point in time.
  - **Soft-state**: data only in memory (not durable)
    - if replica disconnected asynchronously resync
    - if a replica crashes, reboot it to a clean state
    - improved with a distributed cache *or*
    - pass data to  a third party service to persist
  - **Eventually consistent**: after a centrain time all replicas are consistent, but at any given time this might not be the case.
- An "AP system" - "Internet design style"


- **Consequences of BASE**
  - code proves to be much more concurrent hence faster.
  - encourages asynchronous communication
    - send a response to the user before you complete response yourself from elsewhere.
  - elimination of locking and early responses to users make end-user experience fast and positive.
  - so this is a lot more scalable
  - however it is more work and harder for the application developer to implement.
    - in effect we have weakened the semantics of operations and have to code the application to work correctly anyhow
    - the functuiionality given by transactions has to be implemented
    - need to modify rhe end user apllication to mask and asynchronous side effects that might be noticeable.
- But in different classes of applications does it matter?

- **Implementing eventual consistency**
  - Can be implemented with two steps
    - all writes eventually propagate to all replicas
    - writes, when they arrive, are written to a log and applied in the same order at all replicas
  - Therefore consistency does happen (eventually...)
  - Easily done with timestamps.
  - Same technique can be applied to seperate cloud regions during disaster recovery.

---

# CSA, OAuth & OpenID

## Question 4

Discuss the top security threats to/concerns in cloud computing as identified by the Cloud Security Alliance (CSA) in 2016. Explain especially in detail the top three threats.

## Answer 4

- CSA is a non-profit organization that promotes research into and use of best practices for providing security assurance within cloud computing.
  - AWS and MIcrooft etc. are members
- Periodically identifies, collates and ranks the cloud computing industry's top security concerns and threats.
  - latest assessment was in 2016
- Also provides certification, identification of best practices to address the security concerns and threats etc.


**Top three concerns**

- #### Data breaches
  - **Description**
    - An incedent in which sensitive, protected or confidential data is released, viewed, stolen or used by an individual who is not authorised to do so.
    - Data compromise due to improper access controls or weak encryption.
    - Poorly secured data at a greater risk in multi-tenant architectures.
  - **Address threat**
    - Proper data encryption, user credential and backup policies and their enforcement required.
  - **Examples**
    - Anthem's Breach and Ubiquity of comprimised credentials
    - Anti-virus firm BitDefender admits breach, Hacker claims Stolen passwords are Unencrypted
    - TalkTalk criticised for poor security and handling of attack.
- #### Weak identity, credential na daccess management
  - **Description**
    - Data breaches and enabling of attacks can occur nbecause of a lack of scalable identity access management systems, failure to use multifactor authentication, weak password use and a lack of ongoing autoimated rotation of cryptographic keys, passwords and certificates.
  - **Address Threat**
    - Proper encryption and key management; proper implementation of identity, entitlement and access management, use of multi-factor authentication.
    - Meed lifecycle management of identities e.g. immediate de-provisioning of access to resources on personnel changes etc.
  - **Examples**
    - Attackers Scrape Github for Cloud service credentials, Hijack account ti mine virtual currency
    - Dell releases fix for root certificate fail
- #### Insecure APIs
  - **Description**
    - APIs (usually HTTP/REST based) designed to permit access to functionality and data may be vulnerable or improperly utilized exposing applications to attack.
    - Cloud provisioning, management, orchestration and monitoring are also all performed using these interfaces.
    - Exposure of credentials to third-partiest in order to allow API execution.
  - **Address threat**
    - Awareness necessary that reliance on a weak set of interfaces and APIs exposes organizations to a variety of security issues related to confidentiality, integrity, availability and accountability.
    - Proper design required of APIs incorporating up-to-date technology versions and security: identify and access management, encryption and activity monitoring.
  - **Examples**
    - 80% of tested applications not using available security in APIs (e.g. unencrypted traffic and basic authentication) and demonstrated Attackers
      - insecure API implementations threaten clouds
      - web services single sign-on contains big flaws
