# Tutorial 2

## Question 1

NIST defines cloud service delivery models i.e. IaaS, PaaS and SaaS. COmpare and contrast these. Give examples of each from industry cloud vendors.

## Answer 1

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

## Question 2

NIST defines cloud deployment models. COmpare and contrast these.

## Answer 2

- Cloud Deployment Models
  - Public
    - Cloud infrastructure made available to the general public.
    - Ownder by an organisation which is selling cloud services.
  - Private
    - Cloud infrastructure operated solely for an organization.
    - owned or leased by a single organization
    - no public access
    - cloud infrastructure for a single organization only, may be managed by the organization or a 3rd party, on or off premise.
    - A virtual private cloud is a logical subdivision of another cloud (public or private) which looks like an independent private entity.
      - has its own virtualised private network
    - Called a managed cloud if on-premise but managed by a 3rd party.
  - Community
    - Cloud infrastructure shared by several organizations and supporting a specific community
    - shared by several organizations
    - supports a specific community that has shared concerns
    - cloud infrastructure shared by several organizations that have shared concerns, managed by organizations or 3rd party
  - Hybrid
    - Cloud infrastructure composed of two or more clouds that interoperate or federate
    - composition of 2 or more clouds bound by a standard or proprietary technology
    - e.g. to enable data & application portability
    - e.g. to handle cloud bursting: bursts of data and computing surges
      - the application scales from the private to another cloud (typically a public cloud), utilising additional resources of the other cloud type during peak periods through technology


## Question 3

Discuss what is meant by a hybrid cloud. Why is it a popular approach? What is cloud bursting?

## Answer 3

- Hybrid
  - Cloud infrastructure composed of two or more clouds that interoperate or federate
  - composition of 2 or more clouds bound by a standard or proprietary technology
  - e.g. to enable data & application portability
  - e.g. to handle cloud bursting: bursts of data and computing surges
    - the application scales from the private to another cloud (typically a public cloud), utilising additional resources of the other cloud type during peak periods through technology


- 2015 RightScale survey suggests 55% of enterprises planning for hybrid cloud.
  - Why?
    - functionality offered by different cloud platforms varies - need mix and match of “best of breed”
    - tools now exist to control provisioning and scaling in hybrid setups
    - in some cases less expensive to use and also accelerates product time to market
    - Some resource has to be kept local...
      - security and governance restrictions on location of workloads so some has to remain in private cloud
      - legal compliance issues with some types of data e.g. financial/healthcare so some data has to remain in private cloud
    - Security always remains a key concern.

- Migration
  - split and port existing data and workloads
  - needs manual planning, porting and testing in new location
- Interoperability
  - APIs and services allowing communication to integrate clouds and services
  - each cloud may invoke another cloud vendor’s management APIs
  - needs some sort of secure cloud gateway technology
    - manual setup and/or coding of connection technology needed
- Abstraction
  - automated control of hybridisation and the whole hybrid cloud from a higher level which abstracts away from detail of a particular cloud platform technology
    - fully unified view of provisioning and scaling
    - dynamic migration of data and workloads
    - the future really...

- Hybrid cloud interoperability technologies.
  - Built at low level using IaaS e.g. VPN gateways.
    - AWS Direct Connect
  - or use PaaS facility which will transparently map endpoints through a secure channel
    - Azure service bus a good example relaying communication across firewalls and NAT
      - can be used as both a relay and a queue

## Question 4

Describe the web and workers role pattern and discuss why it makes suitable application architecture for cloud applications.

## Answer 4

- A good example of an application architecture suitable for
the cloud.
  - scalable multi-tier application
  - front facing web application
  - separately scaled and independent background processing connected asynchronously with a queue
  - separate scalable data repository
- **Web role**: front facing with IIS; can perform simple tasks
synchronously, but whenever any complex processing is
required it will create a message and drop it into a queue.
(Consists of 1 or more **instances**.)
- One or more **Azure queues** (a type of cloud storage) which
support asynchronous communication between the web
role and the worker role.

- **Worker role**: (consisting of 1 or more **instances**) which pull messages off the queue and perform slow or complex processing tasks.
- **Cloud Storage**: (such as non-relational **Azure tables** and **blobs**) that stores the system’s state and typically includes the result of the worker role’s processing tasks.

## Question 5

What are Azure cloud services, app services and cloud storage?

## Answer 5

### App services

- Originally 3 different types:-
  - Web Apps
  - API Apps
  - Mobile Apps
- As of 2017 there is no difference with respect to hosting them in the cloud and they are differentiated only by the API libraries used in the different types.
- We are using a Web App which is basically a normal ASP.NET web application that has been coded to access Azure cloud storage.
  - so this is deployed as an “App Service”
- Basically deployment to a shared, multitenanted, already running VM instance hosting IIS.
  - exactly the same access to cloud storage
- IIS features to “containerise” i.e. isolate each web application from one another.
  - little admin control (but do you need it?)
- Very fast deployment (seconds) and much lower resource demands as the hosting VMs are already running.

- Background processing available with WebJobs.
  - again “containerised”
  - architecturally simple - looks like a console app if developed in VS
    - can also be uploaded .jar, .php, .js and others
  - event driven e.g. can be woken up when a message appears in a queue

  - default is 1:1 relationship between app service instances and WebJob instances and both may be on same server, but each can also be scaled independently
  - web and worker role pattern still applicable
    - although entities have changed names...
- WebJobs run continuously or at certain times or when a URL is accessed.
  - we will stick to continuous WebJobs and C#


### Cloud Storage

- **BLOB = Binary Large Object**
  - storage of binary data in a bucket which is opaque to the storage service
  - often contains metadata (label on the bucket) that allows one blob to be distinguished from another
  - example of content: videos, songs, pictures…
- **Queues**
  - a classic first-in, first-out data storage structure
  - primarily used for passing data from one role to another in a loosely-coupled fashion
  - generally not used for long-term storage
  - scale really well
- **Non-Relational Tables**
  - schema-less tables containing records (entities) with variable fields with max. size of whole record 1MB
  - database-like features but no referential integrity
  - all has to be handled by programmer
  - scale really well
- **All three**:
  - are a Microsoft product
  - have RESTful APIs
  - are not part of an app service or role instance but are separate entities
- A bit like a “file system for the cloud”
