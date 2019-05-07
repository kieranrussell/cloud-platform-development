# Tutorial 3

## Question 1

In the cloud context what is meant by faults and failures? Is the meaning of “failure” the same when we just talk about individual components? Discuss the characteristics of component failure rate in the real world.

## Answer 1

- AWS go on to say:
  - “Disciplined software engineering can mitigate some of these problems, but ultimately even the most sophisticated software system is dependent on a number of components that are out of its control (e.g., operating system, firmware, and hardware). Eventually, some combination of hardware, system software, and your software will cause a failure and interrupt the availability of your application.”
- So all the time we have a multitude of unavoidable **faults** happening and under these conditions we have to architect our cloud application to try to avoid **failure**.

- Fault tolerance is achieved by designing the system with a high degree of hardware redundancy.
  - but the software has to be designed to use it
    - basically replicated instances of services
- Note that some outages may be caused by upgrades in progress rather
than faults or by hardware or software overloading.
- **eBay**:
  - **“Design to fail”**: implement automated fault detection and a degraded “limp mode”
- The system should be distributed into **fault domains** (or **fault zones**).
  - a **fault domain** is a set of hardware components – computers, switches, and more – that share a single point of failure
  - should separate service instances into separate fault domains thereby improving reliability


## Question 2

Discuss what is meant by availability and an availability level of *“three 9s”*. Calculate how many hours per year this means it is likely to be *unavailable*.

## Answer 2

- Availability is often quoted in *“nines”*.
  - e.g. a solution with an availability level of *"three 9s"* is capable of supporting its intended function 99.9 percent of the time—equivalent to an annual downtime of 8.76 hours per year on a 24x365 basis.


- Gartner suggest:
  - **high availability** = 99.9% availability – **“three 9s”**
  - **normal commercial availability** = 99 – 99.5% availability
    - 87.6 – 43.8 hours/year downtime

**three nines** = 99.9% availability

```
unavailability per year = 365 * 24 * 0.001 = 8.76 hours
```

## Question 3

What is MTBF and MTTR? How do they relate to calculating the availability of a simple service, of a replicated service and of multiple dependent services?


## Answer 3

### MTBF

- mean time between failures
  - really meaning between component faults as defined previously...
- generally used to calculate the probability of failure for a single
solution component
  - average time interval, usually expressed in thousands or tens of thousands of hours that elapses before a component fails and requires service
  - a basic measure of a system’s reliability and is usually represented as units of hours

### MTTR

- mean time to repair
  - the average (expected) time taken to repair a failed component
  - again usually represented as units of hours

### Relate to availability

#### Simple Service
- a measure of the reliability of a service
- ρ is the probability that the service is unavailable either through component failure or network partition
  - calculated as a fraction of the time an individual service instance is unavailable i.e. dimensionless
```
ρ = MTTR /(MTBF + MTTR)
```
- availability of the service is defined as 1 - ρ
- availability as a quantity is popularly given as the equivalent percentage of uptime i.e. 99.5% rather than 0.995


#### Replicated Services

Availability of replicated services
- the *overall* reliability of a system composed of set of replicated, identical and *independent* service instances in a parallel configuration
- **availability** of a system of `n` replicated services is `1 - ρn`
  - where probability of all instances having failed at any instant is `ρn`

#### Dependent Services

Availability of dependent services
- when two (or more) connected services are required to be available
together in order for the system to be available
  - i.e. if *any* of these services fail the system is considered to be unavailable
- aggregate **availability** of a system of connected dependent servicesis
```
(1 - ρi) * (1 - ρj) * (1 - ρk) ...
```
- where `i`, `j` and `k` etc. are the replication factors of the separate subsystems of services as above


## Question 4

A cloud infrastructure hosts a stateless web server system, replicated on three virtual servers (each virtual server is underpinned by a separate real server). The cloud infrastructure service level agreement (SLA) indicates that each virtual server has a mean time between failures of 10 days; a failure on average takes 10 hours to fix. What is the aggregate availability of the cloud-based web server system? Is it providing “three 9s” high availability?

## Answer 4

```
MTBF = 10 * 24 = 240 hours
MTTR = 10 hours

Individual server
----------------------
ρ = MTTR/(MTTR+MTBF)
  = 10/(10+240)
  = 0.04

Multiple machines (x3)
-----------------------
= 1 – ρ^n
= 1 - (0.04)^3
= 0.999936
= 99.9936 %
```

As this availability is greater than 99.9% this service is providing a high availability service as defined by Gartner.

## Question 5

Consider a cloud-based replicated service consisting of a multiple of independent servers. A single server failure happens on average every 4 days and takes 4 hours to fix. The service requires an availability of 0.9984. How many servers are required to offer this level of availability?

## Answer 5

```
MTBF = 4 * 24
     = 96 hours
MTTR = 4 hours

Individual server
---------------------------
ρ = MTTR/(MTTR+MTBF)
  = 4/(4+96)
  = 0.04

Availability for 2 servers
---------------------------
= 1 – ρ^n
= 1 - (0.04)^2
= 0.9984
= 99.84%
```

In order to offer an availability of 99.84% the system would require 2 servers.

The value can be found by trying values 1 - 4 as that is the range you're only realistically going to be asked about.

## Question 6

In a cloud the web and worker role pattern is implemented using 2 web role instances and 2 worker role instances. It also uses cloud storage. Use the data below to calculate the aggregate availability of this system of dependent services (obviously the values are made up for illustration...).

- web role – MTBF = 4 days MTTR = 4 hours
- worker role – MTBF = 4 days MTTR = 4 hours
- cloud storage – MTBF = 4 days MTTR = 2 hours

## Answer 6

#### Web role availability
```
MTBF = 4 days
MTTR = 4 hours

ρ = MTTR/(MTTR+MTBF)
  = 4/(96+4)
  = 0.04

ρ^2 = 1 – ρ^n
    = 1 - (0.04)^2
    = 0.9984
```

#### Woker role availability
```
MTBF = 4 days
MTTR = 4 hours

ρ = MTTR/(MTTR+MTBF)
  = 4/(96+4)
  = 0.04

ρ^2 = 1 – ρ^n
    = 1 - (0.04)^2
    = 0.9984

```

#### Cloud Storage availability
```
MTBF = 4 days
MTTR = 2 hours

ρ = MTTR/(MTTR+MTBF)
  = 2/(96+2)
  = 0.02

ρ = 1 – ρ^n
  = 1 - (0.02)^2
  = 0.98
```

#### Aggregate availability
```
= web ρ * worker ρ * storage ρ
= 0.9984 * 0.9984 * 0.98
= 0.9768
= 97.68%
```
