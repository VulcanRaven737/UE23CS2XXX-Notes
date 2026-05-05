# Cloud Computing — Unit 1: Complete Exam Notes

### UE23CS351B | Dr. Prafullata Kiran Auradkar | PES University

---

## Table of Contents

1. Introduction to Cloud Computing
2. Parallel Computing
3. Grid Computing
4. Cloud Computing Models (IaaS, PaaS, SaaS)
5. Technology Challenges & Business Drivers
6. Cloud Deployment Models (Public, Private, Hybrid, Community)
7. Multi-Cloud
8. Distributed System Models
9. Cloud Architecture
10. IaaS — Infrastructure as a Service (Deep Dive)
11. REST and Web Services
12. PaaS — Platform as a Service (Deep Dive)
13. Communication Using Message Queues (Pub-Sub)
14. SaaS — Software as a Service (Deep Dive)
15. Microservices Architecture
16. Monolithic vs Microservices
17. Challenges of Migrating Monolithic Applications to Microservices
18. Saga Pattern for Distributed Transactions

---

## 1. Introduction to Cloud Computing

### 1.1 What is Cloud Computing?

Cloud computing is the delivery of computing at Internet scale. Compute, storage, and networking infrastructure as well as development and deployment platforms are made available **on-demand** within minutes.

**NIST (National Institute of Standards) Definition:**

> Cloud computing is a model for enabling **ubiquitous** (wherever), **convenient**, **on-demand** (whenever) network access to a shared pool of **configurable computing resources** (e.g., networks, servers, storage, applications, and services) that can be **rapidly provisioned and released** with minimal management effort or service provider interaction.

**Key points:**

- Data centers house the physical compute, storage, and networking infrastructure. When these host cloud resources and enable computing over the Internet, they are called **Cloud Data Centers**.
- Large clouds today often have functions distributed over **multiple locations** from central servers.
- If the connection to the user is relatively close, the server may be designated an **edge server**.

---

### 1.2 Features / Characteristics of Cloud Computing

|Feature|Description|
|---|---|
|**On-Demand Self-Service**|Resources are self-provisioned or auto-provisioned with minimal configuration — no human interaction with the provider needed.|
|**Broad Network Access**|Ubiquitous access from desktops, laptops, and mobile devices is critical.|
|**Resource Pooling**|Cloud services support millions of concurrent users by sharing resources between users to reduce costs.|
|**Scalability**|Cloud services can accommodate larger or smaller loads while supporting QoS expectations like response time.|
|**Rapid Elasticity**|The platform can rapidly increase or decrease computing resources as needed.|
|**Measured Service**|"Pay-as-you-go" — the consumer pays only for the resources actually used by their applications.|

---

### 1.3 Benefits of Cloud Computing

- **Agility:** Quickly spin up resources as needed — compute, storage, databases, IoT, ML, analytics, etc.
- **Elasticity:** Scale resources up or down instantly as business needs change.
- **Cost Savings:** Trade capital expenses (data centers, physical servers) for variable expenses; pay only for what you consume.
- **Deploy Globally in Minutes:** Expand to new geographic regions and deploy globally in minutes.

---

### 1.4 Cloud Computing Usage

Organizations of every type, size, and industry use the cloud for:

- Data backup and disaster recovery
- Email and virtual desktops
- Software development and testing
- Big data analytics
- Customer-facing web applications

**Examples:**

- Healthcare companies → personalized treatments
- Financial services → real-time fraud detection
- Video game makers → online games for millions of players

---

### 1.5 Genesis and Evolution of Cloud Computing

Over the past 60 years, computing has evolved:

- **Centralized computing** (single large computer)
- **Parallel computing** (multiple processors in one system)
- **Distributed computing** (multiple autonomous computers)
- **Cluster computing** (loosely connected machines working as one)
- **Grid computing** (distributed, multi-institutional, heterogeneous)
- **Cloud computing** (on-demand, pay-as-you-go, Internet-scale)

The general computing trend is to leverage **shared web resources** and **massive amounts of data** over the Internet.

**HPC vs HTC:**

|Dimension|HPC (High Performance Computing)|HTC (High Throughput Computing)|
|---|---|---|
|Goal|Maximum performance for a short time|Handle large amounts of computing over long periods|
|Timeframe|Hours/days|Months/years|
|Focus|Speed|Throughput (jobs per unit time)|
|Where used|Science, engineering|Business applications, cloud|

Since 1990, HPC and HTC systems are hidden in **clusters, grids, or Internet clouds** and are employed by both consumers and high-end web-scale computing services.

---

### 1.6 Evolution of the Web (Key Enabler of Cloud)

**Web 1.0:**

1. Static pages
2. Content served from the server's file system
3. Pages built using Server Side Includes (SSI) or CGI
4. Frames and Tables for positioning

**Web 2.0:**

- Focuses on user-generated content, usability, and interoperability
- Also called the "participative social web"
- Uses Ajax and JavaScript frameworks
- Features: dynamic content responsive to user input, APIs for self-usage, information flow via commenting and evaluation

**Web 3.0:**

1. **Semantic Web** — generates and connects content based on meaning of words, not just keywords
2. **Artificial Intelligence** — combined with NLP for smarter search
3. **3D Graphics** — used in museums, games, e-commerce, geospatial contexts
4. **Ubiquity** — content accessible by multiple applications; every device connected to the web

---

### Q&A — Section 1

**Q1: What is the NIST definition of cloud computing? Identify the five key terms in it.** A: NIST defines cloud computing as a model enabling **ubiquitous** (wherever), **convenient**, **on-demand** (whenever) network access to a **shared pool of configurable resources** that can be **rapidly provisioned and released** with minimal management effort.

**Q2: What are the six essential characteristics of cloud computing according to NIST?** A: On-demand self-service, Broad network access, Resource pooling, Scalability, Rapid elasticity, Measured service.

**Q3: What is the difference between HPC and HTC?** A: HPC refers to aggregating computing power for maximum performance for short durations (hours/days) — e.g., solving large scientific problems. HTC focuses on handling large amounts of computing over long periods (months/years) using distributed systems — the model underlying cloud computing.

**Q4: Differentiate Web 1.0, 2.0, and 3.0.** A: Web 1.0 = static pages, server-side content. Web 2.0 = dynamic, user-generated content, participative (Ajax, social media). Web 3.0 = semantic, AI-driven, 3D, ubiquitous access.

**Q5: What is an edge server?** A: When a cloud has functions distributed over multiple locations and the connection to the user is relatively close, that server may be designated an **edge server** to reduce latency.

---

## 2. Parallel Computing

### 2.1 Definition

**Parallel computing** is a type of computation in which multiple calculations or execution of processes are carried out simultaneously.

- **Single system with multiple processors**
- Processors are either tightly coupled with **centralized shared memory** or loosely coupled with **distributed memory**
- Inter-processor communication is through **shared memory** or **message passing**
- **Primary goal:** Increase available computation power for faster application processing and problem solving

---

### 2.2 Types of Parallelism

**1. Bit-Level Parallelism**

- Based on increasing processor word size
- Increasing word size reduces the number of instructions needed for operations on variables larger than the word length
- Example: An 8-bit processor needs 2 instructions to add two 16-bit integers; a 16-bit processor needs only 1 instruction

**2. Instruction-Level Parallelism (ILP)**

- A processor can address less than one instruction per clock cycle
- Instructions can be re-ordered and grouped to execute concurrently without affecting program results
- Pipelining is a form of ILP

Example:

```
for (i=1; i<=100; i=i+1)
    y[i] = y[i] + x[i];
```

Every iteration can overlap with any other iteration — a parallel loop.

**3. Data Parallelism**

- Concurrent execution of the **same task** on each multiple computing core
- Example: To sum an array of size N:
    - Single-core: one thread sums elements [0] to [N−1]
    - Dual-core: Thread A on core 0 sums [0] to [N/2−1]; Thread B on core 1 sums [N/2] to [N−1]

**4. Task Parallelism**

- Concurrent execution of **different tasks** on multiple computing cores
- Example: Two threads each performing a **unique** statistical operation on the same array simultaneously

---

### 2.3 Parallel Computing Architectures

**Multi-core Computing:**

- A processor with two or more separate processing cores, each executing program instructions in parallel
- Each core has its own private cache memory; may be connected via on-chip mesh networks

**Symmetric Multiprocessing (SMP):**

- Two or more independent, homogeneous processors controlled by a single OS instance
- All processors treated equally; connected to a single shared main memory with full access to all common resources

**Massively Parallel Computing:**

- Use of numerous computers or processors to simultaneously execute computations
- Approaches:
    - Grouping processors in a tightly structured, centralized cluster
    - Grid computing: widely distributed computers communicating via the Internet

---

### 2.4 Parallel Computing Software Solutions

**1. Application Checkpointing:**

- Records all current variable states of an application
- Enables restoration and restart from that point in case of failure
- Provides **fault tolerance** for computing systems

**2. Automatic Parallelization:**

- Conversion of sequential code into multi-threaded code
- Allows use of multiple processors simultaneously on an SMP machine

**3. Parallel Programming Languages:**

- **Distributed memory** languages: use message passing to communicate (e.g., MPI)
- **Shared memory** languages: communicate by manipulating shared memory variables (e.g., OpenMP)

---

### 2.5 Distributed Computing

A **distributed system** consists of multiple autonomous computers, each with its own private memory, communicating through a **computer network**.

- Information exchange is accomplished through **message passing**
- Components can be on different networked computers coordinating via HTTP, RPC-like connectors, or message queues
- Significant characteristics: **independent failure of components** and **concurrency of components**

---

### 2.6 Cluster Computing

A **computer cluster** is a set of loosely or tightly connected computers working together so that, in many aspects, they can be viewed as a single system.

- All nodes are set to perform the **same task**, controlled by software
- Mostly have similar hardware configuration and run the same OS (different instances)
- Cluster = low-latency, high-bandwidth interconnected network of standalone computers working as a single integrated computing resource

---

### Q&A — Section 2

**Q1: What is the difference between data parallelism and task parallelism?** A: Data parallelism performs the **same task** on multiple computing cores concurrently (e.g., summing different halves of an array). Task parallelism performs **different tasks** on multiple cores simultaneously (e.g., different statistical operations on the same array).

**Q2: How does bit-level parallelism improve performance?** A: By increasing processor word size, fewer instructions are needed to process variables larger than one word. E.g., a 16-bit processor completes a 16-bit addition in 1 instruction vs 2 instructions for an 8-bit processor.

**Q3: What is application checkpointing?** A: A fault tolerance technique that records all of an application's current variable states at intervals. If the application fails, it can be restored and restarted from the most recent checkpoint rather than from the beginning.

**Q4: Differentiate between SMP and massively parallel computing.** A: SMP = multiple homogeneous processors under one OS with shared memory, equal access to all resources. Massively parallel = use of very many processors/computers (possibly distributed via the Internet) executing computations simultaneously.

**Q5: What is a cluster? How does it differ from a distributed system?** A: A cluster is a set of computers (mostly similar hardware, same OS) working together as a single system where every node performs the **same task**. A distributed system comprises multiple autonomous computers with their own OS/memory that coordinate through message passing — nodes may perform different tasks and have heterogeneous hardware.

---

## 3. Grid Computing

### 3.1 Definition

**Grid computing** is defined as "coordinated resource sharing and problem solving in dynamic, multi-institutional virtual organizations."

- Uses widely distributed computer resources to reach a common goal
- Can be thought of as a distributed system with **non-interactive workloads** that involve many files
- Distinguished from cluster computing in that each node is set to perform a **different task/application**
- Nodes tend to be more **heterogeneous** and **geographically dispersed**

---

### 3.2 Features of a Grid

1. Co-ordinates resources that are **not subject to centralized control**
2. Uses **standard, open, general-purpose protocols** and interfaces
3. To deliver **non-trivial quality of service**
4. Uses a common standard for authentication, authorization, resource discovery, and resource access

---

### 3.3 Virtual Organization (VO)

- A **Virtual Organization (VO)** is a dynamic collection of individuals or institutions from multiple administrative domains
- Used to enable flexible, coordinated, secure resource sharing among participating entities
- A VO forms a basic unit for enabling access to shared resources with specific resource-sharing policies

---

### 3.4 Types of Grids

**Computational Grid:**

> "A hardware and software infrastructure that provides dependable, consistent, pervasive, and inexpensive access to high-end computational capabilities."

- Example: Science Grid (US Department of Energy)

**Data Grid:**

- The storage component of a grid computing system
- Deals with controlled sharing and management of large amounts of **distributed data**
- Provides seamless access to local or remote data required to complete compute-intensive calculations
- Examples: Biomedical Informatics Research Network (BIRN), Southern California Earthquake Center (SCEC)

---

### 3.5 Grid Environment Components

A typical Grid environment has:

- **Grid Information Service** — Collects details of available Grid resources and passes info to the resource broker
- **Resource Broker** — Distributes jobs to Grid resources based on user's QoS requirements
- **Grid Resources** — Cluster, PC, Supercomputer, database, instruments that execute user jobs
- **Grid Application** — The user's computation or data-intensive application

**Flow:** User → Grid Application → Resource Broker (receives info from Grid Information Service) → Grid Resources → Results back to user

---

### 3.6 Grid vs Cloud Computing

|Feature|Grid Computing|Cloud Computing|
|---|---|---|
|Resource sharing|Direct access to resources|Virtualized access|
|Trust model|Trust formed via agreements between domains|Provider-managed security|
|Architecture|Federated, heterogeneous|Centralized data centers|
|Elasticity|Limited|Highly elastic|
|Business model|Academic / research collaborative|Commercial, pay-as-you-go|
|Transparency|User unaware of resource location|User unaware of resource location|
|VO concept|Yes — central to Grid|Not central|
|Deployment model|No clear separation of public/private|Public/Private/Hybrid|

**Compute as a Utility:**

- **Utility computing** focuses on a business model where customers receive computing resources from a paid service provider
- All grid/cloud platforms are regarded as **utility service providers**

---

### Q&A — Section 3

**Q1: How does Grid computing differ from Cluster computing?** A: In cluster computing, all nodes perform the **same task** with similar hardware and OS. In grid computing, nodes perform **different tasks**, are **geographically dispersed**, and are **heterogeneous** (different hardware, OS). Grid also spans multiple administrative domains.

**Q2: What is a Virtual Organization (VO) in Grid computing?** A: A VO is a dynamic collection of individuals or institutions from multiple administrative domains formed to enable flexible, coordinated, secure resource sharing. It defines resource-sharing policies for users from that organization.

**Q3: What are the 4 key features of a Grid?** A: (1) Coordinates resources not under centralized control, (2) Uses standard open protocols and interfaces, (3) Delivers non-trivial QoS, (4) Common standards for authentication, authorization, resource discovery, and access.

**Q4: What is the difference between a Computational Grid and a Data Grid?** A: A Computational Grid provides access to high-end computational capabilities. A Data Grid handles controlled sharing and management of large amounts of distributed data, providing seamless access to data needed for compute-intensive calculations.

**Q5: What is "Compute as a Utility"?** A: A business model where customers receive computing resources from a paid service provider, similar to how utilities like electricity are consumed on demand and charged based on usage.

---

## 4. Cloud Computing Models

### 4.1 Deployment Models

**1. Private Cloud:**

- Computing resources deployed for **one particular organization** — governed, owned, and operated by/for the same organization
- Enjoys cloud benefits (elasticity, scalability, ease of delivery) with access control and security of on-premises infrastructure
- Can be single-tenant from a business perspective but multiple internal users

**2. Public Cloud:**

- Computing resources owned, governed, and operated by a government, academic, or business organization
- Available for **any individual or organization** willing to pay

**3. Hybrid Cloud:**

- Cloud resources used for both B2B (Business to Business) or B2C (Business to Consumer) interactions
- Computing resources bound together by different clouds (mix of on-premises, private, and public)

---

### 4.2 Enabling Technologies for Cloud Computing

1. **Broadband networks and internet architecture** — Internet = network of autonomous cooperative networks interconnected by routers; connectionless packet switching at core
2. **Data center technology** — commodity devices forming the infrastructure
3. **Virtualization technology** — server, storage, and network virtualization
4. **Web technology** — URL, HTTP, HTML, XML
5. **Multitenant technology** — multiple tenants on the same server

---

### 4.3 Service Models (IaaS, PaaS, SaaS)

**Infrastructure as a Service (IaaS):**

- Physical hardware (servers, disks, networks) abstracted into **virtual resources** and allocated
- User manages OS, storage, deployed applications; does NOT manage underlying infrastructure
- Examples: AWS EC2, Azure VMs, Google Compute Engine

**Platform as a Service (PaaS):**

- Provides a **platform** built on top of abstracted hardware for developers to create cloud applications
- User does NOT manage infrastructure, OS, or storage — only manages deployed applications and possibly app hosting environment (e.g., database) configurations
- Examples: Google App Engine, Windows Azure, Elastic Beanstalk

**Software as a Service (SaaS):**

- Provides the **complete application** as a service over the Internet
- User does NOT manage anything — accesses via thin client (web browser)
- Associated data and software are hosted centrally
- Examples: Gmail, Google Docs, Salesforce, Office 365

---

### 4.4 The Stack Layering — Who Manages What?

```
Physical data center
Networking
Storage
Virtualization
Operating System       ← IaaS user manages from here up
Runtime / Middleware   ← PaaS user manages from here up
Application            ← SaaS user manages from here (just configuration)
Data
```

|If user is responsible for...|Then it is|
|---|---|
|OS, runtime, applications|IaaS|
|Only application code & data|PaaS|
|Nothing except usage|SaaS|

---

### 4.5 IaaS — Advantages and Disadvantages

**Advantages:**

- Dynamically choose CPU, memory, storage configuration as needed
- Easy access to vast computing power
- Complete control over software stack

**Disadvantages:**

- Dependent on availability of Internet and virtualization services

**Providers:** AWS, IBM Cloud, Microsoft Azure, Oracle Cloud, Google Cloud

---

### 4.6 PaaS — Advantages and Disadvantages

**Advantages:**

- Easier to develop — focus on code, not infrastructure
- Developer needs only a PC and Internet connection
- Faster time to market

**Disadvantages:**

- Vendor lock-in — moving applications to another PaaS vendor is a problem

**Providers:** Google App Engine (GAE), Windows Azure, Salesforce.com

---

### 4.7 SaaS — Advantages and Disadvantages

**Advantages:**

- Easy to buy — monthly or annual fee
- Less hardware required
- Less maintenance cost
- Almost a no-programming model for end users

**Disadvantages:**

- Totally dependent on Internet connection
- Difficult to switch between SaaS vendors

**Examples:** CRM, Office Suite, Email (Gmail, Outlook)

---

### Q&A — Section 4

**Q1: What is the key difference between IaaS and PaaS in terms of what the user controls?** A: In IaaS, the user controls the OS, storage, deployed applications, and possibly limited networking components. In PaaS, the user only controls the deployed application and possibly the application hosting environment configuration (e.g., database). The provider manages everything else including hardware, virtualization, and middleware.

**Q2: Why is PaaS subject to vendor lock-in?** A: PaaS applications are written for the specific platform provided by the PaaS vendor (specific frameworks, middleware, APIs). Moving the application to another PaaS vendor requires rewriting or significant modification because vendors use different platform standards.

**Q3: How does SaaS charge its users?** A: SaaS uses various models including: subscription (monthly/annual fee), tiered pricing (basic/moderate/superuser levels), usage-based pricing (per API call), per (active) user pricing, per feature pricing, and ad-based revenue.

**Q4: What are the five enabling technologies for cloud computing?** A: (1) Broadband networks/Internet architecture, (2) Data center technology, (3) Virtualization technology, (4) Web technology (URL, HTTP, HTML, XML), (5) Multitenant technology.

**Q5: Name the three cloud service models, their key providers, and which layer of the stack each manages.** A: IaaS (AWS EC2, Azure VMs) — user manages OS upwards; PaaS (Heroku, App Engine) — user manages application code and data; SaaS (Gmail, Salesforce) — user manages nothing except configuration settings.

---

## 5. Technology Challenges & Business Drivers

### 5.1 Technology Challenges

**1. Scalability:**

- Ability to accommodate larger or smaller loads while supporting QoS (like response time)
- Must support scale with a wide range of environments, shared users, and large computing environments
- Facebook maintains ~2 sec response time with 7.5% of global web traffic; Google maintains ~1.7 sec with 5.5% of global web traffic

**2. Elasticity:**

- Actual increasing or decreasing of resources to cope with loads **dynamically**
- Scale Up (Vertical Scaling) — increase power of existing machine
- Scale Out (Horizontal Scaling) — add more machines
- Scale Down — reduce resources; resource allocation and workload placement algorithms required
- Not happening quickly enough impacts QoS or Cost

**3. Performance Unpredictability:**

- Resources are shared among multiple VMs
- Many VMs may run concurrently and affect each other at runtime
- Challenge: How to guarantee **performance isolation**?

**4. Reliability and Availability:**

- Large-scale environments expect hardware failures and software bugs to occur relatively frequently
- Failures can trigger other failures — leading to **avalanche of failures** causing significant outages
- Factors: High number of components, Complexity

**Availability Techniques:**

- **Failure Detection** — detect failed application instances and avoid routing requests to them
    - _Heartbeats:_ Each application instance periodically sends a signal to a monitoring service. If a specified number of consecutive heartbeats are missed, the instance is declared failed.
    - _Probes:_ The monitoring service periodically sends a lightweight service request (probe) to the application. If the instance does not respond to a specified number of probes, it is considered failed.
    - **Trade-off:** Low value for missed heartbeats/probes = fast failure detection but more false positives
- **Application Recovery** — restart failed instances
    - _HTTP Redirection:_ After identifying failed instances, new requests are redirected away from them
    - _Checkpoint/Restart:_ Cloud infrastructure periodically saves application state; on failure, most recent checkpoint is activated and application resumes

**5. Security:**

- Concerns: Violation of confidentiality, data privacy, data leakage and loss
- Data stored and processed by a third-party vendor; location often unknown
- Vast amounts of data travel on networks
- Famous incident: Hacker used Amazon to hack Sony — illustrates shared-cloud risks
- Cloud providers use: identity management, access control, firewalls, encryption, privacy protocols, recovery policies, SLAs

**6. Compliance:**

- Meeting requirements of service users or laws of the country
- Example: HIPAA (Health Insurance Portability and Accountability Act) for healthcare in the USA
- Sarbanes-Oxley 2002 — audit and reporting standards for public companies
- India: SEBI Clause 49 (Corporate Governance Practices)

**7. Multi-Tenancy:**

- A single instance of a component serves multiple tenants/groups of users
- Instances are **logically isolated** but **physically integrated**
- Each tenant gets a dedicated share of configuration and data
- Challenge: Sharing without compromising security

**8. Other Challenges:**

- **Interoperability:** An application on one platform should incorporate services from another platform
- **Portability (Migration):** Moving applications across cloud platforms without changes — not easily possible due to different standard languages used by providers
- **Network Capability and Computing Performance:** High network bandwidth is needed for data-intensive applications, resulting in high cost

---

### 5.2 Business Drivers

**Operational Benefits:**

- Speed to market
- Agility to new requests and need for capacity (Automated)
- Flexibility to support changing scales
- Assurance from specialized cloud providers
- Supports increase of Total Customer Experience
- Different deployment models (Private, Public, Hybrid, Commodity)
- Security

**Cost Benefits:**

- Pay Per Use
- Lower Capital Investment
- Reduced Financial Risk
- Reduced Cost
- Significantly Automated Management
- Expansion of the global customer footprint and business

**Drawbacks:**

- Compliance concerns
- Vendor Lock-In

**Market Opportunity:**

- Worldwide public cloud revenue has grown into hundreds of billions of USD annually
- Organizations compare public vs. private cloud total cost of ownership before committing

---

### Q&A — Section 5

**Q1: What is the difference between scalability and elasticity in cloud computing?** A: Scalability is the ability to accommodate larger or smaller loads while maintaining QoS. Elasticity is about **dynamically** increasing or decreasing resources to cope with loads in real-time — it is the operational mechanism that enables scalability. Elasticity requires resource allocation algorithms and must happen quickly to avoid impacting QoS or cost.

**Q2: Explain heartbeats and probes as failure detection techniques.** A: Heartbeats: Each application instance periodically sends a signal to a monitoring service; missing a specified number of consecutive heartbeats triggers a failure declaration. Probes: The monitoring service sends lightweight requests to the instance; missing a specified number of probe responses triggers failure declaration. There is a trade-off: lower thresholds detect failures faster but may cause false positives.

**Q3: What is the checkpoint/restart technique for application recovery?** A: The cloud infrastructure periodically saves (checkpoints) the state of a running application. If the application fails, the most recent checkpoint is activated, and the application resumes from that saved state rather than restarting from scratch. This is also implemented in middleware like Docker.

**Q4: What is multi-tenancy and why is it a security concern?** A: Multi-tenancy is a mode where a single instance of a component serves multiple tenants/groups of users who are logically isolated but physically share the same infrastructure. The security concern is the risk of data leakage across tenant boundaries — one tenant could potentially access another tenant's data if isolation mechanisms fail.

**Q5: Name three compliance regulations relevant to cloud computing.** A: (1) HIPAA (USA) — protects health information in healthcare, (2) Sarbanes-Oxley Act 2002 (USA) — audit and reporting standards for public companies to prevent accounting fraud, (3) SEBI Clause 49 (India) — corporate governance practices.

---

## 6. Cloud Deployment Models

### 6.1 Public Cloud

**Definition:** A platform or IT model where infrastructure resources located in one or more physical locations are made available as a service in a **pay-as-per-use** model to multiple general public customers by a cloud service provider.

The public cloud is like **renting an apartment** — publicly available resources, available to anyone, accessible over the public Internet on a cost basis.

**Characterizing features:**

1. **Shared Resource Allocation** — users outside the provider's firewall share cloud services and virtual resources
2. **Usage Agreements** — some resources are costed on pay-as-you-use basis; some may be at no cost
3. **Management** — provider maintains hardware, supports the network, and manages virtualization software

**Advantages:**

1. Cost — lower cost due to shared infrastructure; costs are expenses, not capital
2. Less server management — internal teams don't spend time managing servers
3. Time Saving — provider handles maintenance, connectivity, deployment
4. Location Independence — resources exist anywhere
5. Analytics — can perform analytics on high volumes of diverse data types
6. Virtually unlimited scalability — capacity rapidly expands to meet demand

**Disadvantages:**

1. Security — data not stored by the enterprise; multitenancy adds risk
2. Compliance — uncertain if provider complies with security rules
3. Interoperability and vendor lock-in — migrating away is difficult
4. Limited control over infrastructure configurations

---

### 6.2 Private Cloud

**Definition:** A computing model offering a **proprietary environment dedicated to a single business entity**. Also called internal or corporate cloud.

- Enjoys cloud benefits (elasticity, scalability, ease of service delivery) with access control, security, and resource customization of on-premises infrastructure
- Single tenant from a business perspective, but can have multiple internal users
- Can be hosted locally (on-premises) or by a cloud service provider

**Internal vs Hosted Private Cloud:**

- **Internal:** Hardware hosted at a facility owned by the business
- **Hosted:** Cloud service provider provides private resources exclusively to one organization

**Advantages:**

1. More Control — single organization has control over resources and hardware
2. Security — enhanced security with dedicated, physically isolated network
3. Compliance — organizations can comply with strict regulations since data is on hardware inaccessible to others
4. Customization — fully configurable to best suit the organization's performance and agility needs

**Disadvantages:**

1. Cost — large capital outlay required to build
2. Under-utilization — cost of unused capacity falls on the organization (not the provider)
3. Platform scaling — large upward changes require physical infrastructure scaling, which takes time

---

### 6.3 Public Cloud vs Private Cloud (Comparison Table)

|Dimension|Public Cloud|Private Cloud|
|---|---|---|
|Tenancy|Multi-tenant|Single-tenant|
|Infrastructure ownership|Owned by service provider|Owned or hosted by enterprise|
|Connectivity|Public Internet|Private network (intranet)|
|Scalability|Very high|Limited|
|Reliability|Moderate|Very high|
|Management|Provider manages|Managed by enterprise|
|Cost|Cheaper|More expensive|
|Security|Dependent on provider|High-class security|
|Performance|Low to medium|High|
|Servers|Shared|Dedicated|
|Examples|AWS, Google App Engine|Microsoft KVM, HP, Red Hat, VMware|

**Factors influencing the choice:**

1. **Infrastructure** — private cloud = user owns infrastructure (inflexible); public cloud = provider owns infrastructure (elastic, on-demand)
2. **Network bandwidth constraints and cost** — disruptions affect availability; long-term storage may be more cost-effective in private cloud
3. **Control and Security** — businesses with high security standards may prefer private cloud

---

### 6.4 Hybrid Cloud

**Definition:** A cloud environment that uses a mix of on-premises, private cloud, and third-party public cloud services with **orchestration** between these platforms.

- Combines Public and Private clouds to create a **unified, automated, well-managed** environment
- Allows enterprises to deploy workloads in private or public environments and **move between them** as needs and costs change
- **Typically:** Non-critical activities → public cloud; Critical activities → private cloud
- Provides greater flexibility and more data deployment options
- A hybrid cloud workload includes network, hosting, and web service features

---

### 6.5 Community Cloud & Multi-Cloud

**Community Cloud:**

- Shared infrastructure for a specific community of organizations with common concerns (e.g., security, compliance, jurisdiction)

**Multi-Cloud:**

- Using cloud computing services from **at least two cloud providers**
- Environments typically include a combination of two or more public clouds, two or more private clouds, or some combination

---

### Q&A — Section 6

**Q1: What is the key difference between public and private cloud in terms of tenancy?** A: Public cloud is **multi-tenant** — data of many enterprises is stored in a shared environment, isolated by rules and permissions. Private cloud is **single-tenant** — data of a single enterprise is stored on dedicated infrastructure.

**Q2: A company stores patient health records. Which deployment model would you recommend and why?** A: A **private cloud** (or hybrid cloud). Patient health records are subject to strict privacy regulations (e.g., HIPAA). Private cloud ensures the data is on physically isolated hardware inaccessible to others, enabling compliance with regulatory requirements and providing higher security.

**Q3: What is the "apartment" analogy for public cloud?** A: A public cloud is like renting an apartment — the resources are publicly available to anyone, available on a cost basis, and accessible over the public Internet. Just as multiple tenants share a building but have isolated units, multiple cloud users share infrastructure but have logically isolated resources.

**Q4: Define hybrid cloud and describe a use case for it.** A: A hybrid cloud mixes on-premises, private cloud, and public cloud services with orchestration between them. Use case: A bank runs its core transaction processing on a private cloud (for security/compliance) and uses the public cloud for its customer-facing mobile app and analytics workloads.

**Q5: What is the difference between Hybrid and Multi-cloud?** A: **Hybrid cloud** = on-premises/private cloud + public cloud (focus on integration between on-premises and cloud). **Multi-cloud** = multiple cloud providers (focus on diversity across providers, avoiding lock-in). Hybrid emphasizes integration; Multi-cloud emphasizes selecting the best service from each provider.

---

## 7. Multi-Cloud

### 7.1 Definition

A **Multi-cloud architecture** is when an organization uses **multiple public or private cloud services from different providers** in its technology stack.

- Pulls together services from more than one provider to meet an organization's needs
- May integrate IaaS, PaaS, and SaaS in a tightly or loosely coupled architecture
- Seen by CIOs as a good way to avoid lock-in and get the best fit for each workload

---

### 7.2 Advantages of Multi-Cloud

- **Avoid vendor lock-in** — freedom to choose the best service from each provider
- **Cost optimization** — pick the most cost-effective provider for each workload
- **Best-of-breed services** — use each cloud for what it does best
- **Reliability and redundancy** — if one provider has an outage, traffic can shift to another
- **Regulatory compliance** — data can be placed in specific regions/providers for compliance
- Built on open-source technologies like Kubernetes for flexibility and portability
- Works well with DevOps, containers, and microservices architecture

**2025 Multi-cloud context:** AWS US-East-1 outage in October generated 17 million user reports and disrupted thousands of companies globally — illustrating why organizations maintain presence across multiple providers.

---

### 7.3 Multi-Cloud Architecture Patterns

**1. Separate Workloads per Cloud:**

- Different applications or system components run on different clouds
- Each cloud hosts a distinct workload, not replicas; clouds operate mostly independently
- Example: E-commerce website → AWS; Data analytics → Google Cloud; Enterprise apps → Azure
- Why: Organizational separation, cost optimization, regulatory requirements, avoids cross-cloud sync complexity

**2. Active-Active Deployment:**

- Same application runs simultaneously on multiple clouds, serving **live traffic**
- Traffic is distributed across clouds using DNS/load balancing
- Example: App instance on AWS + App instance on Azure, users routed to nearest healthy deployment
- Why: High availability, load distribution, disaster resilience, geographic performance
- Complexity: Very high (data synchronization, consistency issues)

**3. Backup Cloud (Active-Passive):**

- Primary application runs on one cloud; another cloud kept as **standby**
- Secondary cloud activated only during failure
- Example: Primary → AWS; Disaster recovery → Azure
- Why: Disaster recovery, business continuity, lower cost than Active-Active
- Trade-off: Failover time may be minutes or hours; complex to keep passive setup in sync

**4. Service Specialization:**

- Use each cloud for what it does best — no single cloud is best at everything
- Example: AWS → Compute & storage; Google Cloud → AI/ML services; Azure → Identity & enterprise integration

---

### 7.4 Limitations of Multi-Cloud

- **Network complexity** — managing connectivity across multiple clouds
- **Identity management** — need unified IAM, encryption, compliance across providers
- **Data consistency** — keeping data consistent across multiple clouds
- **Monitoring** — unified observability across different cloud provider dashboards
- **Skill requirements and governance** — teams need expertise in multiple platforms
- **Different security models** across providers

**When NOT to use Multi-Cloud:**

- Small teams
- Limited budget
- Simple applications
- Lack of expertise

---

### Q&A — Section 7

**Q1: What is Multi-cloud and how does it differ from Hybrid cloud?** A: Multi-cloud uses cloud services from multiple **different cloud providers** (e.g., AWS + Azure + GCP). Hybrid cloud combines on-premises/private cloud with public cloud. The focus of multi-cloud is **diversity** across providers; hybrid cloud focuses on **integration** between on-premises and cloud environments.

**Q2: Describe the Active-Active multi-cloud pattern and when to use it.** A: In Active-Active, the same application runs simultaneously on multiple clouds serving live traffic, with traffic distributed via DNS/load balancing. Use it when you need maximum availability, load distribution, disaster resilience, and geographic performance improvements. The complexity is very high due to data synchronization and consistency challenges.

**Q3: What is the difference between Active-Active and Active-Passive (Backup Cloud) patterns?** A: In Active-Active, both deployments serve live traffic simultaneously. In Active-Passive, one cloud handles all traffic normally while the secondary cloud remains on standby — only activated during failure. Active-Active has higher availability but much greater complexity; Active-Passive is simpler but has failover delay.

**Q4: Why is identity management a key challenge in multi-cloud architectures?** A: Each cloud provider has its own IAM (Identity and Access Management) system, security models, and encryption methods. Organizations need a **unified** identity and access management strategy that works consistently across all cloud providers, which requires significant architectural effort.

---

## 8. Distributed System Models

### 8.1 Overview

Distributed and cloud computing systems are built over a large number of **autonomous computer nodes** interconnected by SANs, LANs, or WANs.

Depending on application needs, system components in cloud infrastructure can be classified into three types of distributed system models:

**1. Architectural Models**

- **System Architecture:** How components are placed across machines; how responsibilities are distributed
    - Examples: P2P Model, Client-Server Model
- **Software Architecture:** Logical organization of software components and their interactions
    - Example: 3-Tier Architecture

**2. Interaction Models**

- How to handle **time** — are there limits on process execution, message delivery, clock drifts?
- Examples: Synchronous distributed systems, Asynchronous distributed systems

**3. Fault Models**

- What **kinds of faults** can occur and what are their effects?
- Examples: Omission faults, Arbitrary faults, Timing faults

---

### 8.2 Architectural Models

**Peer-to-Peer (P2P) Systems:**

- Every node (peer) acts as **both a client and server**
- Peers join or leave the network autonomously
- No central coordination or central database
- No master-slave relationship
- Self-organizing nodes with distributed control; no peer has a global view of the entire system
- Processing and communication loads distributed across many computers
- Most flexible model but securing is more challenging since each peer has their own data

**Client-Server Model:**

- A set of machines called **servers** offer services to another set of machines called **clients**
- Usually based on simple request/reply protocol — send/receive primitives or RPC (Remote Procedure Calls)
- Client asks for service → server does work → returns result or error code
- Distributes functionality across different machines

**Three-Tier Architecture:**

- Moves client intelligence to a **middle tier** so stateless clients can be used
- Simplifies application deployment
- Most web applications are three-tier

**n-Tier Architecture:**

- Web applications that further forward requests to other enterprise services
- Responsible for the success of application servers

---

### 8.3 Interaction Models

**Synchronous Distributed System:**

Features:

1. Systems share a clock (same clock or different synchronized clocks with known offsets/bounds)
2. Lower and upper bounds on execution time can be set
3. Transmitted messages received within a known bounded time
4. Ordered message delivery — messages delivered in sent order
5. **Lock-step execution** — all nodes processing identical messages do so as soon as received and generate output at the same time
6. Allows assumptions about time and order of events
7. Not very practical in real-world settings

Consequences:

1. Needs a global physical time
2. Needs predictability in timing — can be used for hard real-time applications
3. Safe to use timeouts to detect failures

**Note:** _Clock drift_ refers to a clock not running at exactly the same rate as a reference clock (NTP). After some time, the clock "drifts apart" or gradually desynchronizes.

**Asynchronous Distributed System:**

Features:

1. Clock may not be accurate and can be out of sync
2. No bound on machine/process execution time
3. No bound on message transmission delays — messages can be delayed for arbitrary times
4. No constraints on time and ordering of events
5. Each computer processes independently of others
6. **Most suitable for real-world scenarios**

Consequences:

1. No global physical time — reasoning only in terms of logical time
2. Unpredictable in timing
3. Cannot use timeouts to diagnose issues
4. May use mechanisms like **queues** for asynchronous communication
5. Systems must build algorithms that tolerate different kinds of failures

---

### 8.4 Fault Models

**Terminology:**

- A **failure** occurs when the system cannot meet its promises
- A failure is brought about by an **error** in the system
- The cause of an error is called a **fault**

> **Fault → Error → Failure**

**Types of Faults:**

1. **Transient Faults** — Appear once, then disappear (e.g., a network glitch)
2. **Intermittent Faults** — Occur, vanish, reappear, but with no real pattern — the **worst form of faults**
3. **Permanent Faults** — Once it occurs, only replacement/repair of the faulty component allows the system to function normally

**Purpose of Fault Models:**

1. Faults can occur in both processes and communication channels (software and hardware)
2. Needed to build systems with predictable behavior in case of faults (fault-tolerant systems)
3. A fault-tolerant system functions according to predictions only as long as real faults behave as defined by the "fault model"

**Types of Faults by Category:**

- **Omission Faults** — A component fails to respond or take an action it was supposed to (e.g., a message is not sent/received)
- **Arbitrary (Byzantine) Faults** — A component can fail in arbitrary, unexpected ways — including sending incorrect data
- **Timing Faults** — A component performs its action, but outside the expected time bounds (too early or too late)

---

### Q&A — Section 8

**Q1: Compare P2P and Client-Server architectural models.** A: In P2P, every node acts as both client and server, with no central coordination, no master-slave relationship, and distributed control. In Client-Server, there is a clear division: servers offer services and clients request them, typically via request-reply protocol. P2P is more flexible and decentralized but harder to secure; Client-Server has clear roles and easier management.

**Q2: What is the difference between Synchronous and Asynchronous distributed systems?** A: Synchronous systems have shared/synchronized clocks, bounded execution times, bounded message delays, and allow timeouts for failure detection — but are not practical. Asynchronous systems have no clock guarantees, no bounds on execution or message delay, are more realistic for real-world deployment, but cannot use timeouts and must build fault-tolerant algorithms.

**Q3: What is clock drift and why does it matter in distributed systems?** A: Clock drift is when a clock doesn't run at exactly the same rate as a reference clock, causing it to gradually desynchronize. In distributed systems, clock drift prevents the assumption of synchronized clocks, which is why purely synchronous models are impractical. Systems must account for drift when reasoning about time ordering of events.

**Q4: Explain the difference between transient, intermittent, and permanent faults with examples.** A: Transient faults appear once and disappear (e.g., a momentary network glitch). Intermittent faults occur, vanish, and reappear with no pattern — the hardest to diagnose (e.g., a flaky network cable). Permanent faults require replacement or repair of the faulty component (e.g., a failed hard disk).

**Q5: What is a Byzantine (Arbitrary) fault?** A: A fault where a component fails in an arbitrary, unexpected way — including sending incorrect, conflicting, or malicious data to different nodes. It is the most severe fault type and requires specialized Byzantine fault-tolerant algorithms to handle.

---

## 9. Cloud Architecture

### 9.1 Definition

**Cloud architecture** refers to the way in which technology components making up the cloud environment are engineered or combined to leverage the power of cloud resources to solve business problems.

Cloud architecture defines the **components** as well as the **relationships** between them.

---

### 9.2 Components of Cloud Architecture

**A. Front-end (Client Infrastructure):**

- Client-side interfaces and applications required to access cloud computing platforms
- Examples: Web clients (Chrome, Firefox, IE), thin and fat clients, tablets, mobile devices

**B. Back-end Platform:** The back end is used by the service provider and manages all resources required to provide cloud services:

1. **Application** — Software or platform offered for the client to use
2. **Service** — Software that determines and enables appropriate service access (IaaS/PaaS/SaaS)
3. **Runtime Cloud** — Provides the execution and runtime environment to the VM depending on the service model
4. **Storage** — Huge storage capacity in the cloud for storing and managing data
5. **Infrastructure** — Hardware and software components (servers, storage, network devices, virtualization software)
6. **Management** — Manages and coordinates components in the back end
7. **Security** — Implements security mechanisms for secure access by end-users

**C. Network (Internet/Intranet/Intercloud):**

- Acts as the medium/bridge between frontend and backend
- Establishes interaction and communication between frontend and backend

---

### 9.3 Cloud Platform Design Goals

1. **Scalability** — Scale up, scale out, etc.
2. **Efficiency** — Do work quickly with minimal resource utilization; bring up service quickly across the entire stack
3. **Reliability and Availability**
4. **Simplifying the User Experience**

These are enabled by architectures like:

- Clustering
- Virtualization (Hypervisor)
- Replication
- Elasticity
- Simple deployment and scheduling approaches

---

### 9.4 Layered Cloud Architecture

Cloud architecture can be visualized in three layers:

1. **Infrastructure Layer** — Physical hardware; managed by IaaS
2. **Platform Layer** — Middleware, development tools; managed by PaaS
3. **Application Layer** — Software services; managed by SaaS

---

### 9.5 Market-Oriented Cloud Architecture

As consumers rely on cloud providers for more of their computing needs, they require a specific level of QoS maintained by their providers:

- **Market-oriented resource management** regulates the supply and demand of cloud resources to achieve equilibrium
- **Request Examiner** — ensures no overloading or over-provisioning of resources
- **Pricing Mechanism** — decides how service requests are charged
- **VM Monitor** — tracks availability of VMs and their resource entitlements
- **Dispatcher Mechanism** — starts execution of accepted service requests on allocated VMs
- **Service Request Monitor** — tracks execution progress of service requests
- Multiple VMs can be started and stopped on demand on a single physical machine

---

### Q&A — Section 9

**Q1: Name and describe the three main components of a cloud architecture.** A: (1) Front-end — client-side interfaces (web browsers, mobile devices, thin/fat clients) used to access cloud services. (2) Back-end platform — managed by the service provider; includes application, service, runtime, storage, infrastructure, management, and security components. (3) Network — acts as the bridge (Internet/Intranet) between frontend and backend.

**Q2: What are the four cloud platform design goals?** A: Scalability (scale up/out), Efficiency (fast with minimal resources), Reliability and Availability, and Simplifying the User Experience.

**Q3: What is the role of the Request Examiner in a market-oriented cloud architecture?** A: The Request Examiner ensures there is no overloading or over-provisioning of resources, so that many service requests can be fulfilled successfully within resource constraints.

---

## 10. IaaS — Infrastructure as a Service (Deep Dive)

### 10.1 Programming Model

A **programming model** is an execution model linked into an API or a particular pattern of code. There are two execution models in play:

- The execution model of the **base programming language** (e.g., C — sequential execution)
- The execution model of the **programming model** itself (e.g., cloud environment)

**Standalone programming model:**

- Program needs resources: CPU, Memory, Storage, Network
- Resources made available by the OS
- Everything local on the machine

**IaaS programming model:**

- Resources (CPU, memory, storage, network, IP address) are no longer guaranteed locally
- Must factor in remote execution environment
- A WebServer is needed to accept application requests
- The cloud user can request allocation of **virtual resources** from the IaaS provider

---

### 10.2 IaaS Platform Infrastructure

**Physical Data Centers:**

- IaaS providers manage large data centers globally
- Physical machines with various layers of abstraction made available over the web

**Compute (VMs and Containers):**

|Aspect|Virtual Machines (VMs)|Containers|
|---|---|---|
|OS|Has its own OS|Uses physical server's OS|
|Nature|Looks like actual physical machine to software|Sandboxed process|
|Weight|Heavier|More lightweight|
|Isolation|Strong (full OS isolation)|Process-level isolation|

- Multiple VMs/containers may belong to different users
- Each VM/container is isolated from others
- Achieved via the **hypervisor** (to be studied in Unit 2)

**Storage:**

- Three primary types: Block storage, File storage, Object storage
- **Block storage** pooling example: Virtual disk of 256 GB using 2x128 GB from different physical disks
- Each virtual disk can be attached to a VM
- **Object storage** has become the most common in the cloud — highly distributed, resilient, leverages commodity hardware, accessible over HTTP, essentially limitless scale

**Network (Software Defined Networking):**

- Traditional networking hardware (routers, switches) made available programmatically through APIs
- Each VM has one or more virtual network adapters (V) with IP addresses and DNS names
- Virtual Switch (VS) — completely in software
- Virtual Port (VP) — network adapter invisible to network, only MAC address
- Physical server's network adapter (N) connects to the physical network

---

### 10.3 VM Creation Steps

1. User specifies VM configuration (memory) and software
2. Cloud software:
    - Creates actual VM
    - Creates virtual disk(s) needed
    - Copies OS and app binaries to boot disk
    - Boots OS
    - Sets up virtual network

---

### 10.4 IaaS Advantages (Full List)

- **Flexible** — most flexible cloud computing model
- **Control** — clients retain complete control of their infrastructure
- **Pay-as-you-Go** — no upfront capital expenditures
- **Speed** — provision resources in minutes
- **Availability** — multizone regions provide availability beyond traditional approaches
- **Scale** — seemingly limitless capacity; auto or supervised scaling
- **Latency and Performance** — broad geographic footprint reduces latency

---

### 10.5 When to Use IaaS

- Startups and small companies avoiding hardware/software purchases
- Larger companies wanting complete control but only paying for what they use
- Companies experiencing rapid growth that need scalable infrastructure
- Website hosting — less expensive than traditional web hosting
- Storage, backup, and recovery — handles unpredictable demand
- High-performance computing (HPC) — supercomputers, grids, clusters
- Big data analysis — vast amounts of processing power economically

---

### 10.6 IaaS Limitations

- **Security** — insider threats or system vulnerabilities may expose data between host infrastructure and VMs
- **Multi-tenant security** — vendor must ensure previous customers' data is not accessible on reused storage
- **Internal resources and training** — additional resources needed to effectively manage infrastructure

---

### Q&A — Section 10

**Q1: What is the difference between VMs and Containers in IaaS?** A: VMs have their own OS and look like actual physical machines to software — providing strong isolation. Containers use the physical server's OS, are sandboxed processes, and are more lightweight than VMs. Containers provide process-level isolation while VMs provide full OS-level isolation.

**Q2: What is Software Defined Networking (SDN) in the context of IaaS?** A: SDN is a form of networking in which traditional hardware (routers, switches) is made available programmatically through APIs. In cloud environments, virtual adapters, virtual switches, and virtual ports all exist completely in software, enabling flexible network configuration without physical hardware changes.

**Q3: Why is Object storage preferred for cloud environments?** A: Object storage is highly distributed (thus resilient), leverages commodity hardware, data is accessible over HTTP, and scale is essentially limitless with performance that scales linearly as the cluster grows. It is ideal for unstructured data that needs to be accessed at scale.

**Q4: Describe the 6 steps for creating a VM in IaaS.** A: (1) User specifies VM configuration and software, (2) Create actual VM, (3) Create virtual disks, (4) Copy OS and app binaries to boot disk, (5) Boot OS, (6) Set up virtual network.

---

## 11. REST and Web Services

### 11.1 Service-Oriented Architecture (SOA)

**SOA** defines a way to make software components **reusable via service interfaces**. These interfaces utilize common communication standards so they can be rapidly incorporated into new applications without deep integration each time.

- Each service in SOA embodies code and data for a complete, discrete business function
- Service interfaces provide **loose coupling**
- Services are exposed using standard network protocols (SOAP/HTTP or JSON/HTTP)
- Services are published so developers can quickly find and reuse them

**Two major SOA styles:**

1. **REST** (REpresentational State Transfer)
2. **SOAP-based Web Services**

---

### 11.2 REST — REpresentational State Transfer

REST is an **architectural style** (sometimes also called programming style) for providing a set of rules for building computer systems on the web, making it easier for systems to communicate.

**Key Points:**

- REST helps build client-friendly distributed systems that are simple to understand and simple to scale
- Systems adhering to REST constraints are considered **RESTful**
- RESTful APIs are typically based on HTTP methods to access resources via URL-encoded parameters and use JSON or XML to transmit data
- REST is widely accepted for creating stateless, reliable web APIs

---

### 11.3 REST Design Principles (Constraints)

**1. Client-Server Constraint (Separation of Concerns):**

- The client can change and evolve without changing anything on the server
- Different server aspects can change without breaking clients
- About how the client sends the server a message and how the server responds

**2. Stateless Constraint:**

- Communication between client and server must remain **stateless** between requests
- Each request must contain **all information needed** for the server to answer successfully
- All state information is transferred back to the client as part of the response
- Session state is kept entirely on the **client**
- Benefits: clients are isolated from server changes; promotes redundancy; no synchronization overhead — even if the server fails, the client connects to another server and continues

**3. Cache Constraint:**

- Data in a server's response must be labeled as **cacheable or non-cacheable**
- If cacheable, a client cache may reuse that response data for later equivalent requests
- Improves network efficiency

**4. Uniform Interface Constraint:**

- A uniform interface makes it generic and improves visibility of interactions
- Implementations are decoupled from the services they provide
- Defined by 4 interface constraints:
    - Resource and Resource Identification
    - Manipulation of Resources through Representations
    - Self-descriptive messages
    - Hypermedia as the engine of application state (HATEOAS)

**5. Layered System Constraint:**

- Messages can go through a hierarchy of intermediate components (load balancers, proxies, firewalls)
- Intermediate components should not be able to "see" the next layer
- Client-server interaction should not be affected by these intermediate devices
- All communication remains consistent even if layers are added or removed

---

### 11.4 REST Architectural Elements

**Resources and Resource Identification (URIs):**

- A **resource** is a "Thing" — any information that can be named (document, image, temporal service)
- Each resource is identified by a **URI (Uniform Resource Identifier)**
- URIs provide a global addressing space for resources and facilitate service discovery
- Example: `/pesu/b-block/groundfloor/seminarhall/board/light/1`

**CRUD Operations (HTTP Methods):**

|HTTP Method|Operation|Purpose|
|---|---|---|
|GET|Read|Retrieve a specific resource or collection|
|POST|Create/Partial Update|Create or partially update a resource|
|PUT|Create/Replace|Create or update a resource by replacement|
|DELETE|Delete|Remove a resource|

**Safe and Idempotent Operations:**

|Property|Definition|Example|
|---|---|---|
|**Safe**|Does not modify resources|GET|
|**Idempotent**|No additional effect if called multiple times with same parameters|GET, PUT, DELETE|

GET is both safe and idempotent. POST is neither safe nor idempotent. PUT and DELETE are idempotent but not safe.

---

### 11.5 REST Representations

- A **representation** is a snapshot in time of the state of a given resource
- Made up of data + **representation metadata** (to describe the bytes)
- The data format is known as a **media type (MIME type)** — identifies a specification that defines how a representation is to be processed
- Representations use **hypermedia** — any content containing links to other forms of media
- Navigating hypermedia links is conceptually like browsing web pages by clicking hyperlinks
- All response representations need to be **self-descriptive**

---

### 11.6 Stateless Interactions

Benefits of statelessness:

1. Clients isolated against server changes
2. Promotes redundancy:
    - No need to know which server the client was talking to
    - No synchronization overhead
    - Even if the primary server fails, the client connects to a secondary server and continues

---

### 11.7 Self-Descriptive Messages

- A REST message includes **enough information to describe how to process the message**
- Resources are decoupled from their representation — content can be accessed in various formats (HTML, XML, MIME, plain text, PDF, JPEG, JSON, etc.)
- RESTful systems let the client ask for data in the form they understand
- Metadata about the resource is available for cache control, error detection, authentication, authorization, and access control

---

### 11.8 Important Clarifications about REST

1. **REST is NOT a standard** — it is a design and architectural style for large-scale distributed systems
2. **REST is NOT the same as HTTP** — HTTP can also be used in non-RESTful ways (e.g., SOAP services use HTTP to transport data)

---

### 11.9 Web Services

**Web Service** definition: A software system designed to support **interoperable machine-to-machine interaction** over a network.

- A self-contained, self-describing, modular application designed to be used by other software applications across the web
- Once deployed, other applications can discover and invoke it
- One of the most common instances of an SOA implementation

**Two prominent Web Service implementations:**

1. Simple Object Access Protocol (SOAP)
2. REST

---

### 11.10 SOAP (Simple Object Access Protocol)

- Designed before REST to ensure programs on different platforms/languages could securely exchange data
- Provides structure for transmission of **XML documents** over various Internet protocols (SMTP, HTTP, FTP)

**SOAP Message Structure:**

- **Envelope** — encapsulates all data in the SOAP message
    - **Header element** — contains header information (e.g., authentication credentials)
    - **Body element** — carries the payload of the message
- Payload is marshaled by sender's SOAP engine and un-marshaled at receiver side

**WSDL (Web Service Description Language):** Describes the functionality of the SOAP-based web service

---

### 11.11 Web Services in Action (SOAP Flow)

**UDDI (Universal Description, Discovery, and Integration):** An XML-based registry for businesses to list their web services on the Internet.

**Step-by-step SOAP Web Service Interaction:**

1. Service provider **publishes** its location and description to the UDDI registry
2. Service requester **queries** the UDDI registry using names or identifiers; UDDI returns the WSDL document
3. Service requester reads the WSDL and **forms a SOAP message** binding to all constraints
4. SOAP message is sent to the web service as the **body of an HTTP/SMTP/FTP request**
5. Web service **unpacks** the SOAP request and converts it to a command the application understands and executes
6. Web service packages the response into another **SOAP message** and sends it back
7. Client program **unpacks** the SOAP message to obtain results

---

### Q&A — Section 11

**Q1: What are the 5 REST design constraints and what problem does each solve?** A: (1) Client-Server — separates UI concerns from data storage concerns; (2) Stateless — each request is independent, improving scalability and visibility; (3) Cache — reduces latency and improves network efficiency; (4) Uniform Interface — simplifies and decouples architecture; (5) Layered System — allows intermediaries (load balancers, proxies) without affecting client-server interaction.

**Q2: Differentiate between Safe and Idempotent HTTP operations.** A: A **safe** operation does not modify resources (e.g., GET). An **idempotent** operation has no additional effect if called multiple times with the same parameters (e.g., GET, PUT, DELETE). POST is neither safe nor idempotent. DELETE is idempotent (deleting the same resource twice = same outcome) but not safe (it modifies the resource by removing it).

**Q3: What is the difference between a URI and a URL?** A: A URI (Uniform Resource Identifier) is a string that identifies a resource. A URL (Uniform Resource Locator) is a specific type of URI that also specifies how to locate the resource (includes the access mechanism like http://).

**Q4: Compare REST and SOAP.** A: REST is an architectural style using standard HTTP methods (GET, POST, PUT, DELETE) with lightweight formats (JSON, XML) — simple, stateless, scalable. SOAP is a protocol using XML messages with a strict envelope structure, supports multiple transport protocols (HTTP, SMTP, FTP), and is more complex but provides stricter standards for security and transactions.

**Q5: What is WSDL and UDDI in the context of SOAP web services?** A: WSDL (Web Service Description Language) is an XML-based language that describes the functionality of a SOAP web service — what operations it supports, parameters, and how to call it. UDDI (Universal Description, Discovery, and Integration) is an XML-based registry where service providers publish their service descriptions, enabling service requesters to discover available services.

---

## 12. PaaS — Platform as a Service (Deep Dive)

### 12.1 PaaS Overview

**PaaS** is a complete development and deployment environment in the cloud with resources that enable delivery of everything from simple cloud-based apps to sophisticated cloud-enabled enterprise applications.

- Includes not only abstracted infrastructure (IaaS) but also **middleware, development tools, BI services, database management systems**
- Designed to support the **complete application lifecycle**: building, testing, deploying, managing, and updating
- The hardware and virtual resource mapping is controlled by the PaaS provider
- The cloud user can **configure and build on top** of this middleware (e.g., define a new database table)

---

### 12.2 PaaS Programming Model

Consider a 3-tier photo sharing app:

- **GUI (Front-end):** User uploads photo
- **App Logic (Middle tier):** Face recognition
- **Database (Back-end):** Stores photos and face data

**In IaaS model, you would:**

1. Start a VM
2. Connect block storage
3. Install Web Server, App Server, Database
4. Upload application
5. Run application

**In PaaS model:**

- Upload the application (made up of components/sections)
- Each component specifies its environment (web server or app server role)
- Specify initial number of instances, scale up/down policy
- PaaS Layer handles: deploy, allocate resources, number of instances

In **Windows Azure:**

- **Web roles** — front-end type tasks
- **Worker roles** — tasks that run in the background

---

### 12.3 How PaaS Works Internally

When the developer pushes to the cloud:

- Application with different component indications
- Specification of software (OS, web server) — IaaS portion
- Services needed from cloud (load balancing, database)

The **Cloud Controller** automatically:

- Creates VMs and containers
- Deploys applications
- Connects to services
- Automatically scales up or down

**Storage Services available:**

- Object storage
- NoSQL
- Relational
- Block storage

Applications store state in **Storage Services** — simplifies scaling and recovery.

---

### 12.4 PaaS Advantages

1. **Faster time to market** — no waiting to purchase/install hardware and software
2. **Faster, less-risky adoption** — access to wider variety of OS, middleware, databases, tools than affordable on-premises
3. **Multiple platforms** — develop for multiple platforms (computers, mobile, browsers) more easily
4. **Easy, cost-effective scalability** — scale on-demand by purchasing just the capacity needed
5. **Distributed development teams** — environment accessed over Internet, enabling remote collaboration
6. **Manage complete application lifecycle** — build, test, deploy, manage, update in one integrated environment
7. **Lower costs** — no infrastructure upfront; most PaaS providers charge based on usage
8. **Development framework** — built-in software components for cloud features (scalability, high-availability, multi-tenancy)
9. **Analytics/BI** — tools for analyzing data, finding insights, predicting outcomes

---

### 12.5 PaaS Limitations

1. **Operational limitation (Lack of control)** — platform limits operational capabilities; may affect how PaaS solutions are managed and provisioned
2. **Vendor lock-in** — switching to alternative PaaS options may not be possible without affecting the business
3. **Runtime issues** — PaaS solutions may not be optimized for the language/frameworks of choice; specific framework versions may not be available
4. **Data security** — data residing on vendor-controlled cloud servers poses security risks
5. **Integrations** — complexity of connecting onsite data center or off-premise cloud; existing legacy systems may not be cloud-ready
6. **Customization of legacy systems** — PaaS may not be a plug-and-play solution; customizations may be necessary, leading to complex IT systems

---

### 12.6 PaaS Providers

- Amazon Web Services: **Elastic Beanstalk**
- Microsoft Azure: **Azure DevOps**
- Google Cloud: **Google App Engine (GAE)**

---

### 12.7 Google Colab as a PaaS Case Study

Google Colab is a PaaS example for data science/ML:

- **Cloud-Hosted Environment** — VMs running on Google's servers; no local installation needed
- **Managed Runtimes** — Google manages execution environment with pre-installed Python and ML libraries (TensorFlow, PyTorch, NumPy, Pandas)
- **Access to High-Performance Hardware** — Free access to GPUs and TPUs without purchasing/configuring hardware
- **Interactive Notebook Interface** — Jupyter notebook format combining code, text (Markdown), and visualizations
- **Seamless Collaboration** — Notebooks saved to Google Drive, shareable like Google Docs, real-time collaboration
- **Isolation and Ephemeral Sessions** — Each session runs in an isolated container; data persists only for session duration; VM is recycled afterward

---

### Q&A — Section 12

**Q1: What is the difference between IaaS and PaaS from a developer's perspective?** A: With IaaS, a developer must manually start VMs, install web servers, app servers, and databases, then upload and run the application. With PaaS, the developer simply uploads the application with component specifications; the platform's Cloud Controller automatically creates VMs/containers, deploys the application, connects to services, and scales automatically.

**Q2: What are Web Roles and Worker Roles in Windows Azure (PaaS)?** A: Web Roles handle front-end type tasks (user-facing tasks like serving web pages). Worker Roles handle background tasks (processing jobs, long-running computations). They are two types of PaaS components that specify where code runs in the Azure cloud.

**Q3: Why does PaaS cause vendor lock-in?** A: PaaS platforms are tied to specific middleware, frameworks, languages, and service configurations provided by the vendor. Code is written to work with that platform's APIs and conventions. When the vendor's platform doesn't match future business needs, switching to another PaaS vendor requires significant redevelopment because the underlying platform requirements differ.

**Q4: How does Google Colab exemplify PaaS?** A: Colab eliminates infrastructure management: Google manages VMs, runtime environments, pre-installed libraries (TensorFlow, PyTorch), and even provides GPU/TPU hardware. The user only writes Python code. This matches the PaaS model — user manages application code and data only; everything else is managed by the provider.

---

## 13. Communication Using Message Queues (Pub-Sub)

### 13.1 Communication in Distributed Systems

In distributed cloud environments (client-server, master-slave, P2P), significant interactions between components involve data flows through messages or events.

- Data flows can use protocols: HTTP, AMQP (Advanced Message Queuing Protocol), TCP
- Different interaction styles: synchronous or asynchronous
- In a monolith: simple IPC (Inter-Process Communication) works
- In microservices (different systems): need Inter-Service Communication mechanisms

---

### 13.2 Interaction Styles

**1. Synchronous (Request-Response):**

- The sending system expects an **immediate response**
- Source sends a message and **waits (blocks)** until it receives a response
- A **timeout** is configured so the source stops waiting if no response arrives in time
- Generally results in services being **tightly coupled**

**Types of one-to-one synchronous interactions:**

1. **Synchronous Request/Response** — Client makes a request and waits; results in tight coupling
2. **Asynchronous Request/Response** — Client sends request and doesn't block; service replies asynchronously
3. **One-way notifications** — Client sends a request; no reply is expected or sent

**2. Asynchronous:**

- Client **doesn't block**; response (if any) isn't necessarily sent immediately
- Supports high rates of data flow
- Expectations: Guaranteed delivery, extensive processing, correlation and time series analysis, **decoupling** of source and target

**Types of one-to-many asynchronous interactions:**

1. **Publish/Subscribe** — A client publishes a notification; consumed by zero or more interested services
2. **Publish/Async Responses** — A client publishes a request message and waits for a period for responses

---

### 13.3 Asynchronous Messaging — Advantages

- **Reduced coupling** — sender does not need to know about the consumer
- **Multiple subscribers** — using pub/sub, multiple consumers can subscribe to receive events
- **Failure isolation** — if the consumer fails, the sender can still send messages; messages picked up when consumer recovers; handles intermittent downtime
- **Load leveling** — queue acts as a buffer; receivers can process messages at their own rate

---

### 13.4 Asynchronous Messaging — Disadvantages

- **Infrastructure coupling** — tight coupling with the messaging infrastructure makes switching difficult
- **Latency** — end-to-end latency can be high if queues fill up
- **Complexity** — handling duplicated messages, correlating request-response messages
- **Throughput** — each message requires at least one enqueue and one dequeue operation; queue semantics require internal locking

---

### 13.5 Message Queues

A **message queue** is a form of **asynchronous service-to-service communication** used in serverless and microservices architectures.

**Key properties:**

- Messages stored on the queue until processed and deleted
- **Each message is processed only once, by a single consumer**
- Can decouple heavyweight processing, buffer or batch work, and smooth spiky workloads

**Components:**

- **Producer** — adds a message to the queue
- **Queue** — stores messages temporarily
- **Consumer** — retrieves the message and processes it
- Many producers and consumers can use the queue, but each message is processed by **only one consumer** — this is called **point-to-point (one-to-one) communication**

When a message needs to be processed by more than one consumer, message queues can be combined with Pub/Sub in a **fanout design pattern**.

**Examples:** Apache ActiveMQ, RabbitMQ

---

### 13.6 Publish-Subscribe (Pub/Sub)

**Pub/Sub** is another asynchronous service-to-service communication mechanism.

- Any message published to a **topic** is immediately received by **all subscribers** to that topic
- Can enable **event-driven architectures** or decouple applications to increase performance, reliability, scalability
- Each message delivered to **ALL subscribers** (unlike message queues where only one consumer processes it)

**Example:** Apache Kafka

**Four Core Concepts:**

1. **Topic** — Intermediary channel maintaining a list of subscribers; relays messages received from publishers. Different topic channels exist; different subscribers can look at their interested topics.
2. **Message** — Serialized message sent to a topic by a publisher; publisher has no knowledge of subscribers
3. **Publisher** — Application that publishes a message to a topic
4. **Subscriber** — Application that registers itself with the desired topic to receive appropriate messages

---

### 13.7 Pub/Sub Advantages

1. **Loose coupling** — Publishers never aware of subscribers; both operate independently. Client is no longer concerned whether server processes are running.
2. **Scalability** — Scales beyond capability of a single traditional data center via parallel operations, message caching, tree-based routing
3. **Eliminate Polling** — Push-based delivery; no need for consumers to periodically check for new information
4. **Dynamic Targeting** — Subscribers can change, upgrade, multiply, or disappear; the system dynamically adjusts
5. **Decouple and Scale Independently** — Adding or changing functionality won't send ripple effects across the system
6. **Simplify Communication** — Reduces complexity by replacing all point-to-point connections with a single connection to a message topic

---

### 13.8 RabbitMQ vs Kafka

**Summary:**

- **RabbitMQ** — message broker for task-based asynchronous communication (logically centralized, single broker typically)
- **Kafka** — distributed event-streaming platform for high-throughput data pipelines (natively distributed across multiple brokers)

**Architecture Comparison:**

|Aspect|RabbitMQ|Kafka|
|---|---|---|
|Default mental model|Single broker|Broker cluster|
|Native partitioning|No|Yes|
|Horizontal scaling|Limited|Strong|
|Broker role|Message routing|Log storage|
|Failure handling|Queue replication|Partition replication|

**When to Use Which:**

|Criteria|Use RabbitMQ|Use Kafka|
|---|---|---|
|What is being sent?|Commands / tasks|Events / facts|
|Message lifetime|Short-lived|Long-lived|
|Need to replay data?|No|Yes|
|Throughput|Moderate|Very high|
|Latency|Very low|Low (but higher than RabbitMQ)|
|Consumers|One service processes a message|Many systems consume same event|
|Ordering requirement|Per queue|Per partition|
|Failure handling|Immediate retries, DLQ|Reprocessing via offsets|

---

### 13.9 Redis

**Redis** (Remote Dictionary Server) — a fast, open-source, **in-memory key-value data store** for use as:

- Database
- Cache
- Message broker
- Queue

**Why use Redis?**

- All Redis data resides **in-memory** — eliminates disk access, avoids seek time delays
- Can access data in **microseconds**
- Ideal for use cases requiring extremely low latency

---

### Q&A — Section 13

**Q1: What is the fundamental difference between message queues and publish-subscribe systems?** A: In a **message queue**, each message is processed by **only one consumer** (point-to-point). In a **pub/sub system**, each message published to a topic is received by **all subscribers** to that topic. Message queues are for task distribution; pub/sub is for event broadcasting.

**Q2: What are the advantages of asynchronous messaging over synchronous messaging?** A: Asynchronous messaging provides: reduced coupling (sender doesn't need to know about consumer), support for multiple subscribers, failure isolation (if consumer fails, sender still sends messages and they are queued), and load leveling (queue acts as a buffer for spiky workloads). Synchronous messaging creates tight coupling and fails immediately if the recipient is unavailable.

**Q3: When would you choose Kafka over RabbitMQ?** A: Choose Kafka when you need: very high throughput, events/facts that need to be stored long-term, ability to replay data, many systems consuming the same event stream, and partition-level ordering. Use RabbitMQ for low-latency task/command distribution where one service processes each message and replay is not needed.

**Q4: What is load leveling in the context of message queues?** A: Load leveling is when a queue acts as a buffer between producers and consumers, allowing consumers to process messages at their own rate regardless of how fast producers send messages. This smooths out spiky workloads — during traffic spikes, messages accumulate in the queue and are processed steadily, preventing the consumer from being overwhelmed.

**Q5: What is the fanout design pattern?** A: The fanout pattern combines message queues with Pub/Sub to deliver a single message to multiple consumers. A publisher sends to a topic; the topic routes the message to multiple queues; multiple consumers each read from their own queue — enabling both guaranteed delivery (queue semantics) and multiple consumers (pub/sub semantics).

---

## 14. SaaS — Software as a Service (Deep Dive)

### 14.1 SaaS Overview

**SaaS** is a way of delivering applications as a service over the Internet — instead of installing and maintaining software, you simply access it via a web browser.

**SaaS Functioning:**

- Cloud service provider (AWS, Azure, IBM Cloud) manages the cloud environment
- Users interact via web browser or mobile, or use APIs (like REST) to connect the software to other functions
- Takes advantage of **multitenant architecture** using pooled resources
- Software updates, bug fixes, and maintenance taken care of by the SaaS provider

---

### 14.2 SaaS Examples

|Category|Examples|
|---|---|
|Office apps|Google Docs, Sheets, Office 365|
|Email|Gmail, Outlook|
|File Storage|DropBox, OneDrive|
|Social Media|Facebook, Snapchat|
|CRM|Salesforce|
|Customer support|Zendesk|

---

### 14.3 SaaS Characteristics

**1. Multi-tenant Architecture:**

- All users and applications share a single, common infrastructure and code base (centrally maintained)
- SaaS providers can make upgrades more often with less customer risk and lower adoption cost
- Easier to roll out new versions and reduce time maintaining outdated code

**2. Easy Customization:**

- Each user can customize applications to fit their business processes without affecting the common infrastructure
- Customizations are unique to each company and preserved through upgrades
- Most SaaS apps are preconfigured plug-and-play products

---

### 14.4 SaaS Revenue Models

|Model|Description|
|---|---|
|Subscription|Pay monthly rental|
|Tiered Pricing|Basic, Moderate, Superuser levels|
|Usage-based pricing|Pay for API calls made|
|Per (Active) User Pricing|Pay for number of users|
|Per Feature Pricing|Each feature priced separately|
|Ad-based Revenue|Pay per click|

---

### 14.5 SaaS Disadvantages

1. **Security** — data stored in the cloud; security concerns for some users
2. **Latency** — data/applications stored at variable distance from end-user; potential greater latency vs. local deployment; not suitable for millisecond-response-time applications
3. **Dependency on Internet** — most SaaS applications unusable without Internet
4. **Switching vendors is difficult** — transferring very large data files over Internet and converting/importing them to another SaaS is slow and difficult

---

### 14.6 SaaS Architecture

In a SaaS architecture:

- **Front-end/GUI** — browser-based
- **Business Logic** — runs on backend cloud infrastructure
- **Business Logic** — frequently implemented as **microservices**

**Example — Building a Book Mart SaaS Application:**

1. Build browser-based GUI (what it looks like, what technology to use)
2. Identify features: authentication, listing books, search, shopping cart, payment, inventory refresh
3. Identify backend workflows and customization of order of display
4. Identify features as microservices (RESTful API): Login, List Books, Search, Shopping Cart, Purchase
5. Identify non-functional requirements: availability, scaling, multi-tenancy, security

---

### 14.7 IaaS vs PaaS vs SaaS — Key Differentiators

**IaaS — For Maximum Control and Flexibility:**

- Best for: Migrating legacy apps (lift-and-shift), custom backend systems, unpredictable workloads
- Key drivers: Need to manage OS/middleware/runtime; custom security and compliance; in-house DevOps expertise; HPC or big data with custom hardware tuning

**PaaS — For Rapid Development and Innovation:**

- Best for: Startups, rapid application development, DevOps teams
- Key drivers: Fast time to market; minimal operational overhead; automated scaling; lower staff management costs

**SaaS — For Turnkey, Low-Maintenance Solutions:**

- Best for: Non-technical, everyday business applications (CRM, email, HR)
- Key drivers: Zero maintenance; lower predictable subscription costs; easy cross-device accessibility; minimal customization needs

---

### Q&A — Section 14

**Q1: What is the multi-tenant architecture of SaaS and why is it important?** A: Multi-tenant architecture means all users/customers share a single infrastructure and codebase that is centrally maintained. It is important because it enables SaaS providers to make upgrades more frequently with less risk, reduces maintenance of multiple code versions, and lowers the adoption cost for customers.

**Q2: What are the main SaaS revenue models?** A: Subscription (monthly/annual fee), Tiered Pricing (different feature sets at different prices), Usage-based (per API call), Per Active User (per user count), Per Feature (each feature priced separately), Ad-based Revenue (pay per click).

**Q3: Why is SaaS not suitable for applications that require millisecond response times?** A: In SaaS, data and applications are stored in the cloud at a variable distance from the end-user, introducing network latency. For applications requiring sub-millisecond or very low latency response times, this additional network round-trip makes SaaS unsuitable. Local deployment would be required.

---

## 15. Microservices Architecture

### 15.1 What are Microservices?

**Microservices** are small, independent services that work together and are components or processes of an application.

**Martin Fowler's Definition:**

> The microservice architectural approach develops a single application as a **suite of small collaborating services**, each running in its own process and communicating with lightweight mechanisms, often an HTTP resource API. These services are built around **business capabilities** and independently deployable by fully automated deployment machinery. There is a bare minimum of centralized management; services may be written in **different programming languages** and use **different data storage technologies**.

**Key properties:**

- Distributed and loosely coupled
- One team's changes won't break the entire app
- Each microservice has its own process, its own database
- Communicate via lightweight protocols (HTTP/REST)

---

### 15.2 Benefits of Microservices Architecture

1. **Flexibility** — Different microservices can use different tech stacks; smaller codebase makes tech upgrades easier; can incrementally adopt newer technology
2. **Reliability** — If one feature goes down, the entire application doesn't go down; fix the corresponding microservice and deploy immediately
3. **Development Speed** — Smaller code volume; faster for new team members; IDE faster; shorter startup time; higher developer productivity
4. **Building Complex Applications** — Break complex applications into independent components; each component can be further broken down into independent tasks
5. **Dynamic Scalability** — Each microservice can be scaled individually; smaller size makes caching more effective
6. **Continuous Deployment** — To update one component, only that particular microservice needs to be redeployed

---

### 15.3 Microservices Principles

- **Single Responsibility Principle** — Each microservice has only one responsibility and cannot serve more than one at a time
- **Modelled Around Business Domain** — Each service focuses on a specific business capability; technology stack chosen to best solve that business purpose
- **Isolate Failure** — A service can fail at any time; detect failure quickly and automatically restore
- **Infrastructure Automation** — Scripting environments (configuration management) to apply the same configuration across thousands of nodes
- **Deploy Independently** — Platform agnostic; can be designed and deployed independently without affecting other services

---

### 15.4 API Gateway in Microservices

- Clients cannot call services directly
- **API Gateway** acts as an **entry point** for clients to forward requests to microservices

Advantages:

- All services can be updated without clients' knowledge
- Services can use messaging protocols that are not web-friendly
- API Gateway provides cross-cutting functions: security, load balancing, rate limiting, authentication

---

### 15.5 Microservices Limitations

- **Building** — upfront time to identify dependencies; completing one build may trigger several others; consider effects on data
- **Testing** — Integration and end-to-end testing is more difficult; a failure in one part can cause failures a few hops away
- **Versioning** — Updating to new versions may break backward compatibility; multiple live versions add maintenance complexity
- **Deployment** — Needs automation investment; complex orchestration of service rollout order
- **Logging** — Distributed systems need log management; scale makes individual log management hard
- **Monitoring** — Centralized view needed to pinpoint sources of problems
- **Debugging** — Remote debugging through local IDE isn't an option across dozens of services
- **Connectivity** — Must consider service discovery (centralized or integrated)

---

### 15.6 Microservices Examples

**Uber's Architecture:**

- Separate microservices for: user service, driver service, trip service, notification service, payment service, location service, etc.
- Each service scales independently; service failures don't bring down the whole platform

**Twitter's Architecture:**

- Separate microservices for: tweet service, user timeline service, home timeline service, search service, notification service
- Fan-out service propagates tweets to followers' timelines

---

### Q&A — Section 15

**Q1: How does the Single Responsibility Principle apply to microservices?** A: Each microservice should have only one responsibility — it serves one specific business function and nothing more. This ensures services remain small, focused, and independently deployable. If a service starts doing multiple things, it should be split into separate microservices.

**Q2: What role does an API Gateway play in microservices architecture?** A: The API Gateway is a single entry point for all client requests. It routes requests to the appropriate microservice, provides cross-cutting concerns (security, authentication, load balancing, rate limiting), and shields clients from the internal service topology. Services can be changed or replaced without clients knowing.

**Q3: What are the main challenges of testing microservices?** A: (1) Integration testing is more complex — testers must understand all interacting services; (2) Microservices are often asynchronous — hard to deterministically test timing-dependent interactions; (3) A failure in one service can cascade to cause failures in dependent services; (4) End-to-end testing requires all services to be available simultaneously.

**Q4: Why does microservices architecture improve reliability compared to monolithic architecture?** A: In a monolithic app, a bug in one module can crash the entire application. In microservices, if one service fails, only that service is affected — the rest of the application continues running. The failed service can be fixed and redeployed independently without affecting other services.

**Q5: Why is continuous deployment easier in microservices?** A: Since each microservice is independently deployable, updating one feature only requires redeploying that specific microservice. In a monolith, any change requires rebuilding and redeploying the entire application. Microservices enable smaller, faster, less risky deployments.

---

## 16. Monolithic vs Microservices Architecture

### 16.1 Monolithic Architecture

**Definition:** An approach of building an application where **all features are in a single codebase**, typically with a single database.

- All components share the same resources and memory space
- Traditional way of building applications
- Includes: client-side UI, Business Logic, Data Access Layer, and Database — all bundled together
- Characterized by large codebase, long release cycles, and large teams

**Typical Challenges:**

- Changes are not easily integrated (whole system may need rebuilding)
- Scalability challenges
- New technology adoption is difficult
- Difficult to adapt to newer practices or devices
- Reliability can be a concern

**Strengths of Monolithic Architecture:**

1. Development is simple
2. Testing is simple — launch application and start end-to-end testing; automation with Selenium is straightforward
3. Deploying is straightforward — copy packaged application to server
4. Scalability is simple initially — add a new instance and configure load balancer

**Important:** Many of the most successful and largest applications were initially developed and deployed as monoliths. Monolithic architecture worked successfully for many decades.

---

### 16.2 SOA vs Microservices

|Dimension|SOA (Service Oriented Architecture)|Microservices Architecture|
|---|---|---|
|Architecture approach|"Share-as-much-as-possible"|"Share-as-little-as-possible"|
|Importance|Business functionality reuse|Bounded context / Single Responsibility|
|Governance|Common governance and standards|People, collaboration, freedom of options|
|Communication|Enterprise Service Bus (ESB)|Simple messaging systems|
|Message protocols|Multiple message protocols|Lightweight protocols (HTTP/REST)|
|Threading|Multi-threaded with overhead|Single-threaded, Event Loop for non-blocking I/O|
|Reuse vs. Decoupling|Maximizes application service reusability|Focuses on decoupling|
|Database|Traditional relational databases|Modern relational databases|
|System changes|Modify the monolith|Create a new service|
|DevOps|Becoming popular, not mainstream|Strong focus on DevOps / Continuous Delivery|

**Note:** Microservice architecture is generally considered an **evolution of SOA** — services are more fine-grained and function independently of each other.

---

### 16.3 Monolithic vs Microservices Summary

|Dimension|Monolithic|Microservices|
|---|---|---|
|Structure|Single unified unit|Collection of smaller independent units|
|Codebase|Single large codebase|Each service has its own codebase|
|Database|Single shared database|Each service has its own database|
|Deployment|Entire application redeployed|Individual service redeployed|
|Scaling|Scale entire application|Scale individual services|
|Technology stack|One technology stack|Different stacks per service|
|Team structure|Large teams working on same code|Small teams owning specific services|
|Release cycle|Long|Short and frequent|
|Failure impact|One bug can crash entire application|Failure contained to one service|

---

### Q&A — Section 16

**Q1: When would you still prefer a Monolithic architecture?** A: Monolithic is preferred when: the application is small and simple, the team is small, the business requirements are straightforward and unlikely to change dramatically, rapid initial development is required without the overhead of distributed systems, and when operational simplicity is valued over scalability.

**Q2: What is the main architectural difference between SOA and Microservices?** A: SOA follows a "share-as-much-as-possible" approach using an Enterprise Service Bus (ESB) for communication and focuses on business functionality reuse. Microservices follow a "share-as-little-as-possible" approach using simple messaging (HTTP/REST), focus on bounded context/single responsibility, and have a strong emphasis on decoupling and independent deployment.

---

## 17. Challenges of Migrating Monolithic Applications to Microservices

### 17.1 Why Migrate to the Cloud?

**Benefits of cloud migration:**

- **Scalability** — better ability to scale up or down based on requirements
- **Cost** — pay for what you use; no costly data centers
- **Integration** — seamlessly connect systems; alleviates hardware refresh risks
- **Access** — data accessible regardless of physical machinery status
- **Security** — cloud providers build with privacy and security in mind; automatic security updates

**The Problem with Monolithic Architecture on Cloud:**

- Monolithic apps are not designed to utilize all cloud benefits
- Single codebase; single database; all components tightly coupled
- Challenges: not easily integrated, scalability issues, etc.

---

### 17.2 What is Migration?

**Cloud migration** = moving critical services and applications from on-premise/collocated hardware to AWS, GCP, Azure, or other cloud providers.

**Application migration** = moving software applications from one computing environment to another (e.g., on-premises server → cloud provider's environment).

---

### 17.3 The 6R Migration Strategies

|Strategy|Also Known As|Description|
|---|---|---|
|**Re-host**|Lift-and-Shift|Migrate to cloud without changes. Fastest and easiest. Limited cloud benefits.|
|**Re-platform**|Lift-Tinker-and-Shift|Basic architecture stays the same; minor optimizations (e.g., managed DB via Amazon RDS).|
|**Re-architect**|Re-design|Systematic and aggressive remodeling; highest ROI; exploits full cloud nativity.|
|**Re-purchase**|N/A|Move perpetual licenses to SaaS model (e.g., CRM to Salesforce.com)|
|**Retire**|N/A|Remove features no longer in use (post-migration design strategy)|
|**Retain**|N/A|Keep only features critical to the application (post-migration design strategy)|

**Re-architecting architectures:**

- Monolithic → SOA
- Monolithic → Microservices (reaps all cloud features — truly **cloud-native**)
- SOA → Microservices

---

### 17.4 Migration Suitability Table

|Dimension|Re-hosting|Re-platforming|Re-architecting|
|---|---|---|---|
|Application Suitability|Web-compatible UI, MVC architecture, Flask/.NET/Symphony; HTML/PHP apps not ideal|Web-compatible UI, MVC architecture, must allow easy DB connection; Traditional XAMPP-like servers suitable|Monolithic but web-compatible; large codebases; resource-intensive; desktop apps with frameworks NOT suited|
|Business Suitability|Higher network speed; low technical debt; small apps needing temp boost|Database hosting; improve DB scalability; boost fault tolerance; more resilient data|Looking for large improvements in availability, testability, continuous delivery, reusability, lower infrastructure costs|

---

### 17.5 Key Migration Challenges

**1. Service Decomposition:** Identifying possible candidates that could represent a microservice.

Guidelines:

1. Stop adding more things to the monolith; fix what is broken; accept only small changes
2. Find **seams** in the monolith — components more loosely coupled than others — and use them as starting points
3. Identify **low-hanging fruits** — components where business units want more advanced features

Once decomposed, more moving parts create operational and infrastructural overheads (configuration management, security, provisioning, integration, deployment, monitoring). **Containerization** helps reduce these complexities.

---

**2. Persistence — Monolithic Database Decomposition:**

A monolithic app has a single database; microservices need multiple databases (one per service).

Three common patterns to handle:

**A. Reference Tables:**

- Function accesses a table belonging to another function (joins between modules)
- Example: Order module uses reference to Products table
- Solutions:
    - **Data as an API** — the owning microservice exposes its data via API
    - **Projection/Replication of data** — copy/replicate relevant data to the consuming service

**B. Shared Mutable Data:**

- Multiple modules access the same table to maintain shared state
- Example: Order, Payment, and Shipping all use the same `ShoppingStatus` table
- Solution: Model as a **separate microservice** that manages this shared state

**C. Shared Table:**

- A database table is modeled with attributes needed by two or more modules
- Example: `Products` table serving both Product and Inventory functionalities
- Solution: Split the table into **individual entities** that belong to the specific bounded context of each separate microservice

---

**3. Tackling Transaction Boundaries:**

With a monolithic app and single database, **ACID properties** (Atomicity, Consistency, Isolation, Durability) can be guaranteed with row locks.

Moving to microservices = **database per service** = distributed transaction problem.

Two solutions:

**A. Two-Phase Commit (2PC):**

- A **controlling node** houses most logic; **participating nodes** perform the actions
- **Phase 1 (Prepare):** Controlling node asks all participating nodes if ready to commit; each responds yes or no
- **Phase 2 (Commit):** If all respond yes → controlling node tells them to commit. If even one responds no → controlling node tells all to roll back.

**B. Compensating Transactions (Saga Pattern):**

- A **saga** is a sequence of local transactions
- Each service performs its own transaction and **publishes an event**
- Other services listen to that event and perform the next local transaction
- If one transaction fails, the saga executes **compensating transactions** to undo the impact of preceding transactions

---

**4. Performance:**

The increase in resource usage may cause a microservices-based application to execute slower.

Solutions:

1. Introduce additional servers
2. Log performance data; analyze at a later point to detect problems
3. Implement: throttling, service timeouts, dedicated thread pools, circuit breakers, asynchronous programming

---

**5. Testing:**

- Integration tests are challenging — QA engineers must know all interacting services
- Microservices-based applications are generally **asynchronous** — difficult to write deterministic tests
- Solutions: Adopt various testing methodologies, leverage CI capabilities through automation, standard agile methodologies

---

**6. Inter-Service Communication:**

In a monolith: components invoke each other through method/function calls. In microservices: each service runs in isolation → must use **IPC (Inter-Process Communication) mechanisms**.

Challenges:

- Remote invocation is subject to network failures, timeouts, partial failures
- Must design for failures in communication
- Must consider patterns: how services interact, how to handle failures

---

### 17.6 Generic Steps for Monolithic to Microservices Migration

```
1. Understand the features of the monolith
       ↓
2. Identify independent components based on functionality and 
   dependency on the central database tables
       ↓
3. Identify points of interaction and gateways to concretize on components
       ↓
4. Containerize the application and convert logical components to 
   functional microservices
       ↓
5. Connect the created microservices through API calls
       ↓
6. Test the application
```

---

### 17.7 Common Challenges of Re-Architecting

- Identification of services based on the overall application's functionality
- Ensuring appropriate connection and communication between services
- Conversion of large and bulky desktop applications is challenging
- Identifying the boundary between re-architecting and rebuilding entirely
- Requires more effort
- Operational complexity increases — managing and monitoring many services
- Coordinating rapidly changing services necessitates automated CI/CD

---

### 17.8 Attributes for Comparing Migration Approaches

1. Performance
2. Scalability
3. Throughput
4. Accessibility
5. Load time
6. Cloud Nativeness

---

### Q&A — Section 17

**Q1: What are the 6R migration strategies? Which has the highest ROI and why?** A: Re-host (lift-and-shift), Re-platform (lift-tinker-and-shift), Re-architect (redesign), Re-purchase (move to SaaS), Retire (remove unused features), Retain (keep critical features). **Re-architecting** has the highest ROI because it exploits complete cloud nativity, enables microservices/SOA benefits, and unlocks maximum scalability, availability, and continuous delivery — though it requires the most effort.

**Q2: What is the "seams" concept in service decomposition?** A: Seams are parts of a monolithic application where components are already more loosely coupled than the rest. These natural boundaries are good starting points for decomposition into microservices because they require fewer changes to existing code to isolate and extract as independent services.

**Q3: Explain the Two-Phase Commit (2PC) protocol.** A: 2PC has a controlling node and participating nodes. In **Phase 1 (Prepare)**, the controlling node asks all participants if they are ready to commit — each responds yes or no. In **Phase 2 (Commit)**, if all respond yes, the controlling node tells all to commit; if even one responds no, the controlling node tells all to roll back. This ensures atomicity across distributed databases.

**Q4: What is the Saga Pattern and how does it differ from 2PC?** A: The Saga Pattern is a sequence of **local transactions** — each service performs its own local transaction and publishes an event; other services listen and perform the next transaction. If a transaction fails, **compensating transactions** undo preceding transactions. Unlike 2PC (which uses locking and requires all participants to agree before committing), Saga is **eventually consistent** — it is asynchronous, more resilient, and avoids distributed locking — making it more suitable for microservices.

**Q5: What is the "Shared Mutable Data" problem in database decomposition?** A: When multiple modules of a monolith access the same table to maintain shared state (e.g., Order, Payment, and Shipping all updating a `ShoppingStatus` table), moving to microservices requires this shared state to be modeled as a **separate microservice** that exclusively owns and manages that data, exposing it via APIs rather than direct database access.

**Q6: Differentiate between Re-hosting and Re-platforming.** A: **Re-hosting (Lift-and-Shift)** migrates the application to the cloud without any changes — fastest, easiest, but doesn't leverage cloud-native benefits fully. **Re-platforming (Lift-Tinker-and-Shift)** keeps the basic architecture but makes minor optimizations (e.g., migrating from a self-managed DB to Amazon RDS managed DB) — gains some cloud benefits like managed database scaling without rewriting the application.

---

## 18. Saga Pattern for Distributed Transactions

### 18.1 The Problem

In a microservices architecture with **database-per-service**, standard ACID transactions cannot span multiple services because each service manages its own database. Traditional 2PC can work but has limitations:

- Single point of failure (the controlling node)
- Long-held locks on resources
- Not well-suited to highly distributed, asynchronous microservices

---

### 18.2 What is a Saga?

A **Saga** is a design pattern for managing distributed transactions as a **sequence of local transactions**, each of which updates the database and publishes a message or event to trigger the next local transaction.

**Two types of Saga coordination:**

1. **Choreography** — Each service publishes domain events that trigger local transactions in other services (decentralized — no central coordinator)
2. **Orchestration** — A central orchestrator tells each service what local transactions to execute

---

### 18.3 Compensating Transactions

If a step in the saga fails:

- The saga executes **compensating transactions** to **undo** the changes made by preceding successful transactions
- Compensating transactions are the "rollback" mechanism in a saga

**Example — Order Processing Saga:**

1. Order Service: Create order → publishes "OrderCreated" event
2. Payment Service: Reserve payment → publishes "PaymentReserved" event
3. Inventory Service: Reserve inventory → publishes "InventoryReserved" event
4. Shipping Service: Create shipment → publishes "ShipmentCreated" event

If Step 3 (Inventory) fails:

- Inventory Service publishes "InventoryReservationFailed"
- Payment Service compensates by releasing/refunding the reserved payment
- Order Service compensates by canceling the order

---

### 18.4 Saga vs 2PC

|Dimension|Two-Phase Commit (2PC)|Saga Pattern|
|---|---|---|
|Coordination|Centralized (controlling node)|Decentralized (choreography) or centralized (orchestration)|
|Consistency model|Strong (ACID)|Eventual consistency|
|Locking|Holds locks across all participants|No distributed locks|
|Failure handling|Rollback all or commit all atomically|Compensating transactions|
|Suitable for|Tightly coupled, low-latency systems|Loosely coupled microservices|
|Scalability|Lower (blocking, locking)|Higher (non-blocking, async)|
|Complexity|Lower implementation complexity|Higher (must design compensating logic)|

---

### Q&A — Section 18

**Q1: Why is the Saga pattern preferred over 2PC in microservices?** A: Microservices are loosely coupled, independently deployed, and often use asynchronous communication. 2PC requires distributed locking and a centralized coordinator — creating tight coupling and a single point of failure that contradicts microservices principles. Saga is non-blocking, asynchronous, and more resilient: each service manages its own local transaction and compensating logic, making it better suited to distributed microservices environments.

**Q2: What is a compensating transaction in the Saga pattern?** A: A compensating transaction is the "undo" operation for a previously successful local transaction in a saga. If a later step fails, compensating transactions are executed in reverse order to undo the changes made by the preceding successful steps, restoring the system to a consistent state.

**Q3: What is the difference between Choreography and Orchestration in Saga?** A: In **Choreography**, there is no central coordinator — each service listens for events and decides on its own what to do next, publishing events that trigger other services. In **Orchestration**, a central Saga Orchestrator sends commands to each participating service, telling it what local transactions to execute and tracking the overall saga progress.

---

## Quick Reference Summary Table

|Topic|Key Concept|One-Line Summary|
|---|---|---|
|Cloud Computing|NIST Definition|Ubiquitous, on-demand access to shared configurable resources|
|Cloud Characteristics|6 Features|On-demand, Broad access, Pooling, Scalability, Elasticity, Measured|
|Parallel Computing|HPC vs HTC|HPC = fast bursts; HTC = sustained throughput over months/years|
|Bit-level Parallelism|Word size|Larger word → fewer instructions for wide operations|
|Data Parallelism|Same task|Same operation on different data partitions simultaneously|
|Task Parallelism|Different tasks|Different operations on same data simultaneously|
|Grid Computing|VOs|Multi-institutional, heterogeneous, geographically distributed|
|IaaS|Virtual resources|User manages OS upwards; provider manages hardware|
|PaaS|Platform|User manages application + data; provider manages everything else|
|SaaS|Complete app|User manages nothing; accesses via browser|
|Public Cloud|Multi-tenant|Shared infrastructure, pay-as-you-go, provider-managed|
|Private Cloud|Single-tenant|Dedicated infrastructure, higher security, higher cost|
|Hybrid Cloud|Mix|On-premises + private + public with orchestration|
|Multi-Cloud|Multiple providers|Different providers for different workloads/redundancy|
|P2P|No central control|Every node is both client and server|
|Client-Server|Request-reply|Clear separation of service providers and consumers|
|Sync. Distributed|Bounded time|Clock-synchronized, timeouts possible, not practical|
|Async. Distributed|No time bounds|No clock sync, queues used, real-world default|
|REST|Stateless API|Uniform interface, stateless, cacheable, layered|
|SOAP|XML protocol|Envelope/body, WSDL, UDDI, works over HTTP/SMTP/FTP|
|Message Queue|Point-to-point|Each message processed by exactly one consumer|
|Pub-Sub|Broadcast|Each message delivered to all subscribers|
|RabbitMQ|Task queue|Commands, low latency, short-lived, single broker|
|Kafka|Event streaming|Events, high throughput, replay, distributed by design|
|Monolith|Single codebase|All features together; simple but hard to scale/change|
|Microservices|Independent services|Each service has own DB, process, and deployment|
|API Gateway|Entry point|Routes client requests; provides cross-cutting concerns|
|Re-host|Lift-and-shift|No changes, fastest migration, limited cloud benefits|
|Re-platform|Lift-tinker-shift|Minor optimizations, some cloud benefits|
|Re-architect|Redesign|Full cloud nativity; highest ROI; most effort|
|2PC|Distributed transactions|Prepare + Commit phases; atomic across all participants|
|Saga|Compensating txns|Local transactions + compensating rollbacks; eventual consistency|

---

_End of Unit 1 Notes — Cloud Computing (UE23CS351B)_