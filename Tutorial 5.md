# Tutorial 5

## Question 1

Discuss the technical approaches to virtualization. Discuss the three approaches to virtualization on x86 processors.

## Answer 1

Cloud uses same basic concepts as virtualization on the desktop.
- on desktop provides convenient user experience e.g. other OS, drag & drop, firewalls, DHCP, NAT, sandboxed, elevated privileges, legacy software, user can configure and manage VMs...
- Presents logical view of physical resources.
  - pool physical resources and manage them as a whole
  - dynamically created at time of need
  - not just OS instances but also storage, networking...


- The three types of virtualization are:
  - **binary translation**
    - Binary Translation or “Full Virtualization”
    - User level instructions run unmodified at native speed.
    - Guest OS kernel does not need to be altered but runs in Ring 1.
    - Guest kernel completely decoupled from the underlying hardware by the virtualization layer.
      - hypervisor in Ring 0
      - hypervisor will emulate a set of virtualised devices
    - The hypervisor translates all guest privileged instructions on the fly to new sequences of instructions that have the intended effect on the virtualised devices.
      - these new sequences of instructions run in Ring 1 and call the hypervisor
      - performance penalty due to translation and longer code
        - speeded up by caching translation results for reuse
  - **paravirtualization**
    - Lower runtime virtualization overhead than binary translation.
    - Again user level instructions run unmodified at native speed.
      - Guest kernel in Ring 1; hypervisor in Ring 0; again user level instructions run unmodified at native speed
    - However guest OS kernel modified to replace privileged instructions with hypercalls that communicate with the hypervisor.
      - having to modify the kernel for specific hypervisors is not a good business option...
    - The hypervisor also provides hypercall interfaces for other critical kernel operations such as memory management, interrupt handling and time keeping.
  - **hardware-assisted**
  - Current generation CPUs now have hardware support :-
    - hardware-assisted virtualization
    - new privilege mode Ring -1 for hypervisor “above” Ring 0
    - second-level address translation (SLAT): optimisations to allow guest VMs to more efficiently access host virtual memory tables
    - e.g. Intel VT and AMD-V extensions
  - Guest kernels will have been written to run in Ring 0 and here can be deployed unaltered.
    - no need for binary translation
    - hypercalls used only to execute admin/management functions of hypervisor



## Question 2

Differentiate between Type 1 and Type 2 hypervisors. What is used in the cloud? What is Hyper-V and how does it relate to MS Azure?

## Answer 2

### Type 1

- Used in the cloud – also called native or baremetal hypervisors.
  - no host OS kernel
    - just hypervisor + guest kernels
  - hardware assisted virtualization
    - i.e. hypervisor in Ring -1
  - Hyper-V, VMWare ESXi, KVM, XenServer...
- Much faster than Type 2 as fewer software layers
i.e. one kernel.
- Not usually a full desktop experience – client
version of Microsoft Hyper-V a key exception.
  - Type 1 hypervisors really for servers
- Live VM migration possible.
- Needs separate management interface to manage VM lifecycle perhaps with privileged access.
- No host OS to exploit so more secure.
- Management more likely possible through scripts.
- MS Hyper-V underpins Azure in data-centres.
- Supported hardware more limited than Type 2.
  - hypervisor inseparable from computer set up and will need full software rebuild to remove

### Type 2
Sometimes called **hosted** or **embedded** hypervisors.

- Type 2 hypervisors are now confined to desktop virtualization software.
  - e.g. VMWare Workstation, VMWare Fusion, VirtualBox
  - full host OS installed and used
  - attractive as a simple and clean user experience with simple VM installation and creation utilities
    - really for desktops/laptops
  - libraries of pre-built VM images available
  - originally just used binary translation but some features of hardware assisted virtualization (memory management) now incorporated


- VMWare Workstation components include a Ring 0 hypervisor and Ring 3 user application on the host.
  - all installed on top of exiting host OS as a normal install
- Some performance penalty due to having both host and guest kernels.
  - this is a key issue for cloud deployment as they run with too much overhead
- Almost universal hardware compatibility.
  - runs on anything that can run Windows or Linux as a host OS

### Hyper-V

- As is usual in this type of architecture Hyper-V introduces a management VM instance (“root partition”). This provides amongst other things:-
- Virtualization Stack to manage lifecycles of guest VMs etc.
- VMBus providing optimised communication between guest VMs (“partitions”)
- Integrates with **Windows Azure Services for Windows Server VM Cloud** facilitating datacentre VM provisioning and management.

Microsoft Hyper-V however is also incorporated into current Windows desktop OSs as an optional feature for Type 1 desktop virtualization.
  - smaller feature set than the server version
  - its main use is for software testing and demonstration especially on different MS OS versions

## Question 3

Compare and contrast virtualization and containers.

## Answer 3

Significantly smaller and more efficient than creating dedicated VMs.
- anecdotally a server host utilising containers can effectively host 4-6 times more server instances than a dedicated VM approach
  - Characterised by:-
  - strong isolation at an OS process level
  - fast start and lower overall resource requirements
  - standard application packaging (if used in conjunction with a technology like Docker)
  - one or more add-on management systems offering more features including packaging and deployment, clustering, orchestration, load balancing...

- **Containers** run as sandboxed user processes sharing common kernel instance and common libraries with other containers. A container sees itself as its own independent system.
- Two current types running different OS's native applications:-
  - **Linux Containers** (LXC)
  - **Windows Containers**
  - Windows 10 & Windows Server 2016
  - see:- http://containerjournal.com/2016/10/28/linux-vs-windowscontainers-whats-difference/
- Containers are the enabling technology for microservices discussed in a later lecture.

Two OS enhancements required to underpin containers:-
- to manage and monitor control of allocation of resources to containers e.g. CPU, memory, bandwidth etc.
- Linux Containers: **Control Groups** (Cgroups)
- Windows Containers: **Windows Job Objects**
  - to isolate containers’ execution environments from each other. This allows containers to have their own process (and process id space), root file system, network devices (IP address etc.), user id space etc.
  - Linux Containers: **Kernel Namespaces**
  - Windows Containers: **Object Namespaces**


## Question 4

Discuss container technology and its relation to cloud computing including
orchestration. Discuss some current technologies and orchestration products.

## Answer 4

### Docker

- Built on top of containers.
  - a container management tool
  - for managing both Linux and Windows Containers
- Ready to run “packaged-up” applications.
  - portable deployment across machines by offering an abstraction of machine-specific settings
  - bundle all dependencies together
  - can be run on any Linux distribution or Windows Server instance as appropriate in a virtualized sandbox without the need to make custom builds for different environments
- Provides development tools to assemble container from source code with full control over application dependencies.
- Also provides reuse, versioning (similar to git), sharing, API...

### Cluster Management (Orchestration)

- VM resource allocation under control of cloud vendor infrastructure and is largely opaque to developer.
  - see Resource Management slides later
- Container resource allocation in contrast has evolved with container technology and there are lots of competing add-on options.
  - different sets available as services by different cloud vendors
  - see summary table on following slide

- Features include (and vary):-
  - describe and deploy/delete containers given a description (YAML) of application
    - manage deployments (run rolling updates, replace a current application Docker image individually or in batches)
  - create services
    - scale deployments up and down (load balancers)
    - health monitoring and recovery
    - live (hot) migration
  - metrics, persistence, resource sharing and security
  - service discovery
  - monitor application
