# Module: Azure compute and networking services
# Key notes

To actually design anything in Azure you gotta understand the compute and networking stack like its your own toolbox. VMs, functions, containers, VNets, subnets, routing they all snap together like a puzzle that decides how fast, secure and reliable your whole setup runs.

**Bottom line:**
Build smart now, because Future You aint trying to debug a busted network at 2 AM wondering why a VM cant talk to anything.

## Table of Contents
- [Azure virtual machines](#azure-virtual-machines)
- [Azure virtual desktop AVD](#azure-virtual-desktop-avd)
- [Azure containers](#azure-containers)
- [Azure functions](#azure-functions)
- [Application hosting options](#application-hosting-options)
- [Azure virtual networking VPN](#azure-virtual-networking-vpn)
- [Azure ExpressRoute](#azure-expressroute)
- [Azure DNS](#azure-dns)

# Azure virtual machines

**Key points**
* Provides IaaS in a form of virtualized server
* Fully customize all software and configs without physical hardware
* Can scale for redundancy, availability and load

**Scale sets**
* Manage groups of identical, load balanced VMs
* Ideal for compute heavy, big data and container workloads

**Availability sets**
* Protection agains planned (think updates) and unplanned (think power, network failures)
* Use update domains (UD) to stagger reboots
* Use fault domains (FD) to seperate VMs across independent power, network racks
* No extra cost
```pgsql
Availability Set
 ├─ Fault Domain 1
 │   ├─ Update Domain 1
 │   └─ Update Domain 2
 └─ Fault Domain 2
     ├─ Update Domain 1
     └─ Update Domain 2
```

**Takeaway**

VMS = raw control

Scale sets = auto scaling

Availability sets = HA within a datacentre

# Azure virtual desktop (AVD)

**Key points**
* Virtualized Win desktops and apps delivered from Azure
* Works on nearly any device, OS or location
* Built on Microsoft Entra ID for secure access and identity

**Enhanced security**
* Centralized identity, MFA
* RBAC for precise user permissions
* No date stored on local devices
* Supports isolated singel or multi sessions

**Multi session Win 10/11**
* Enables multi users on single VM
* Better performance and app compatibility than Windows server RDS

**Takeaway**

AVD = secure and scalable Windows desktop without managing desktop fleets

# Azure containers

**Key points**
* Lightweight app packaing, no OS management needed
* Fast startup, high density, great for microservices
* Quick restarts during crashes or changes to hardware
* Supports docker fully

**Azure container instances (ACI)**
* Fastest way to run containers with zero IaaS
* Pure PaaS, upload and run

**Azure container apps (ACA)**
* Simplifies container deployments, scaling and load balancing.
* Event driven, elastic design (Think Helen from incredibles)

**Azyre kubernetes service (AKS)**
* Full container orchestration
* Manages scaling, updates, self healing (Think rejuvenation potion from diablo II) and deployment pipelines
```scss
Containers
 ├─ ACI (simple)
 ├─ ACA (scalable microservices)
 └─ AKS (full orchestration)
```
**Takeaway**

Containers = microservices. ACA/ACI = simple: AKS = enterprise level

# Azure functions

**Key points**
* Serverless, event driven compute
* No VM, no container, no IaaS to manage
* Trigger by events (HTTP, timers, queues, Karen, etc)
* Auto scales instantly and you only pay while code runs

**Stateless**
* Function restarts fresh every time

**Stateful**
* Uses durable functions to carry state between executions
```vbnet
Event → Stateless Function → Done

Event → Stateful Function
      ↳ Saves context → Next run
```
**Use cases**
* REST endpoints
* Scheduled tasks
* Message processing
* Background logic

**Takeaway**

Functions = code only focus, Azure handles scaling, infra and execution (KA POW!)

# Application hosting options

**Azure app service**
* Host web apps, APIs, background jobs, mobile backends
* Automatic scaling, high availability
* Supports Windows and a penguin named TUX
* Continuous deployment from Github, DevOps or an repo

**Sercive types**
* **Web apps**
* **API apps**
* **WebJobs**
* **Mobile apps**

**Built in advantages**
* Handles load balancing
* Traffic manager
* Secure endpoints
* Fast scaling
* No underlying VM management
* Multi language support (ASP.NET, Java, Node, Python, PHP, Ruby)

**Takeaway**

App service = fully managed web hosting with CI/CD and automatic scaling built in

# Azure virtual networking (VPN)

**Key points**
* Azure VPN is your on prem network extended into Azure
* Connects VMs, databases, services, apps and on prem systems
* Supports both public(internet facing) and private(internal) endpoints

**Core capabilities**
1. **Isolation and segmentation**
* Create fully isolated VNets
* Define private IP ranges
* Split VNets into subnets for organized seperation
* Internal and external DNS support

**Takeaway**

You control your own IP space and how its segmented

2. **Internet connectivity**
* Public IP: accessible from internet
* Public load balancer: controlled exposure

**Takeaway**

You choose what is public and what stays internal. Red pill or blue pill Neo?

3. **Communication between azure resources**
* VNet based comms support VMs, AKS, App service environments, scale sets
* Service endpoints secure access to SQL, storage and other azure services

**Takeaway**

Internal traffic stays secure and optimized

4. **Hybrid connectivity**
* Point to site VPN: individual client: Azure
* Site to site VPN: on prem firewall: Azure gateway
* ExpressRoute: private fiber link, no internet usage

**Takeaway**

You can extend your on prem network directly into Azure at 3 levels

5. **Routing control**
* Azure auto routes between subnets and networks
* Route tables (UDR) override defaults
* BGP propagates routes from on prem to Azure

**Takeaway**

Architect defines traffic flow, not Azure

6. **Traffic filtering**
* NSG (Network security groups): inbound/outbound rules at L4
* NVA (Network virtual appliances): full firewall appliances inside Azure

**Takeaway**

NSG = basic firewall: NVA = enterprise firewall

7. **VNet peering**
* Directly link VNets in same or different regions
* Traffic stays on Microsofts backbone, never internet
* Enables global network topologies

**Takeaway**

Peering builds fast, private, global Azure networks

# Azure ExpressRoute

**Key points**
* Private dedicated connection between on prem and Microsoft cloud
* Traffic never touches the public internet giving more stability and security
* Uses an ExpressRoute circuit provided through a connectivity partner
* Supports Azure Microsoft 365 and Dynamics 365

**Why it matters**
* High reliability stable latency and enterprise grade hybrid setups
* Strong choice for regulated environments datacenter extension and mission critical workloads

**Core benefits**

1. **Private connectivity**
* Traffic flows on the Microsoft private backbone instead of the public internet
* Avoids instability packet loss and random routing events

**Takeaway**

ExpressRoute = private fiber straight into Azure

2. **Global reach**
* Connects your on prem sites through the Microsoft global network
* Example Asia office to Europe datacenter without touching the public internet
```graphql
On-prem
   │
   │  ExpressRoute Circuit
   ▼
Microsoft Backbone Network
   │
   ▼
Azure
```
**Takeaway**

Global Reach = private global site to site network without VPN chaos

3. **Dynamic routing BGP**
* Uses BGP for automatic route exchange between your network and Azure
* Keeps routing fresh updated and clean without manual work

**Takeaway**

BGP keeps hybrid routing automatic and stable

4. **Built in redundancy**
* Every peering location uses redundant hardware
* Multiple circuits possible for extra high availability

**Takeaway**

Redundancy built in so you can sleep without stress

**Connectivity models**
* CloudExchange colocation request virtual cross connect
* Point to point Ethernet direct private link
* Any to any WAN integration Azure becomes part of your WAN
* ExpressRoute Direct connect straight into Microsoft peering sites with ten or one hundred gig throughput

**Takeaway**

Choose the model that fits your network design

**Security considerations**
* Traffic avoids the public internet which reduces exposure
* Some services like DNS checks certificate checks and CDN may still use public paths

**Takeaway**

ExpressRoute protects the main transport path but not every last request

# Azure DNS

**Key points**
* DNS hosting service running on Microsoft Azure global infrastructure
* Provides name resolution for Azure resources and external resources
* Managed with the same Azure credentials portal CLI and APIs
* Supports public and private DNS zones

**Reliability and performance**
* Hosted on global Azure DNS servers for strong availability
* Uses anycast routing so the closest DNS server answers first
* Gives faster and more resilient name lookups worldwide
```pgsql
Public DNS Zone  → Internet clients
Private DNS Zone → VNet clients only
```

**Takeaway**

Azure DNS = fast reliable global name resolution without extra hardware

**Security**
* Built on Azure Resource Manager
* RBAC controls who can change DNS zones or records
* Activity logs help with audits and issue tracking
* Resource locks prevent someone from deleting important DNS zones by accident

**Takeaway**

RBAC and locks keep you safe from accidental DNS disasters

**Ease of use**
* Manage DNS through portal PowerShell CLI REST API or SDK
* Integrates smoothly with other Azure services
* Same billing support and identity as the rest of Azure

**Takeaway**

DNS management works exactly like everything else in Azure clean simple and unified

**Private DNS**
* Lets you use custom internal domain names inside Azure VNets
* Helps you avoid long Azure provided hostnames
* Perfect for internal services and hybrid deployments

**Takeaway**

Private DNS keeps internal names clean consistent and invisible to the public

**Alias records**
* DNS record that points directly to an Azure resource
* Updates itself if the resource IP changes
* Removes manual edits when your app scales or shifts

**Takeaway**

Alias records = DNS that updates itself and always stays correct

## References
https://learn.microsoft.com/et-ee/training/modules/describe-azure-compute-networking-services/
# **Important!**
* Azure DNS cannot purchase domain names
* Buy domains through App Service Domains or a third party
* After purchase you can host and manage DNS in Azure DNS
