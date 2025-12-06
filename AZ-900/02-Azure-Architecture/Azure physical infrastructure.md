# Describe Azure physical infrastructure

## Table of contents
* [Azure physical infrastructure](#azure-physical-infrastructure)
* [Regions](#regions)
* [Availability zones](#availability-zones)
* [Region pairs](#region-pairs)
* [Sovereign regions](#sovereign-regions)

## My interprentions

## Azure physical infrastructure

Physical infra for azure starts with datacenters. They are facilities with resources arranged in racks that have dedicated power, cooling and networking infrastructure. Azure has these globally however they arent directly accessible. Additionally datacenters are grouped into regions and availability zones that are designed to help you achieve resiliency and reliability for your ciritical business workloads.

### Why does it exist?

They exist because they are the foundational physical locations required to house IT infrastructure that ultimately runs every single digital service in the world. Including but not limited to the internet and public cloud itself.

### What does it do?

It houses a large scale IT infrastructure that is powered, cooled and connected to network. Additionally its also protected by physical security.

### How does it work?

It works by combining power system, compute and storage, cooling system, networking and connectivity with security and fire suppression that are all interconnected systems into one secure facility that guarantees that the IT equipment inseide can operate reliably, efficiently and without interruption 24 hours a day.

### Summary

Physical data centers are essentially big IT houses that are self sufficient that store all digital data that are being put into it and additionally runs all the digital services.

### Regions

Region is a geographical are on the planet that contains at least 1 but potentially multiple data centers that are nearby and networked together with a low latency network. Azure assigns and controls the resources within each region to ensure workloads are appropriately balanced.

### Why does it exist?

They exist to provide global deployment options, ensure high availability, satisfy regulatory compliance needs and minimize latency for customers around the world. Basically the foundation for cloud providers to build their services.

### What does it do?

They serve as infrastructure by hosting physical data centers and are designed to deliver servives that meet high availability, low latency and regulatory compliance needs.

### How does it work?

They work by grouping together one or more physically seperate data centers aka availability zones and connecting them with high speed, low latency network. This in return creates a large resilient and unified geographical unit for deploying cloud services.

### Summary

Regions are used to host at least 1 or more data centers to provide global deployment options, provide users with high availability, reliability and compliance needs aka the foundation.

## Availability zones

Physically seperate data centers within Azure region. Each Availability zone is made up of 1 or more data centers equipped with independent power, cooling and networking. Its set up to be an isolation boundary. If one goes down, the other continues working, thus ensuring your data and services are safe. They are also connected wtih high speed fiber optic networks.

### Why does it exist?

To provide users with high availability and fault tolerance withing a single cloud region. Basically key mechanism that allows users to build apps that can survive a failure of an entire data center without suffering downtime.

### What does it do?

Provide high availability and fault tolerance by allowing applications to withstand data center failures within a single cloud region.

### How does it work?

They work because they are physically isolated from each other but not too far apart and connected via high speed but low latency network ensuring that you dont have to suffer any downtime.

### Use cases

In order for availability zones to actually be of any use you would need to set up your own redundancy i.e duplicating your resources to be replicated in another availability zone.

Availability zones are primarily for VMs, managed disks, load balances and SQL databaes. 
Services that support AZ fall into three categories which are:

* Zonal services: You pin a resource to specific zone (VM, managed disk, IP address)
* Zone redundant services: Platform replicates automatically across zones (zone reduntant storage, SQL database)
* Non regional services: Services are always available from Azure geographies and are resilient to zone wide outages as well as region wide outages

### Summary

Availability zones consist of seperate data centers, each one is made up of at least 1 data center or more. They are set up to be isolation boundary, meaning they are not in 1 place but still connected to ensure data moves between them. If one goes down the other continues working.

## Region pairs

Most regions are paired with another region within the same geography such as US, Europe, Asia. They are at least 482 KM apart. This allows for the replication of resources across a geography that in return helps reduce the likelihood of interruptions because of events like natural disasters, civil unrest, power outages or even a meteor strike. Basically anything that would affect an entire region.

### Why does it exist?

It provides disaster recovery capabilities and minimizes risk during planned system updates by linking 2 geographically  seperated regions in a synchronized manner.

### What does it do?

They provide an enhanced framework for geo redundancy and disaster recovery, almost the same principle as with AZ zones but with regions, if 1 falls, the other continues working.

### How does it work?

It works by creating a pre defined, non physical logical link between seperate azure regions, typically withing the same geopolitical to manage and coordinate planned maintenace, prioritized recovery and automated geo redundancy services.

### Additional advantages

* If an extensive outage occurs, one region out of every pair takes priority to make sure at least one is restored as quickly as possible for applications hosted in that region pair.
* Updates are rolled out to paired regions one region at a time to minimize downtime and risk application outage.

### Summary

Basically region pairs is like backup but instead of the data copy its the replication of the same system just in a different region. If one region pair fails another takes over. Additionally updates and maintenance are always done 1 region pair at a time, to ensure there is no downtime and risk outage.

### Sovereign regions

Azure also has sovereign regions, that are instances of Azure which are isolated from the main instances. These are primarily used for compliance or legal purposes.

### Why does this exist?

Sovereign regions exist mostly if you need them for compliance or legal purposes

### What does it do?

Ensures that all data and operations withing that region are exclusively subject to the legal jurisdiction and governance structures of a specific country or geopolitical area.

### How does it work?

It implements strict controls over the physical location, operational management and legal governance of cloud infra. Isolating it from foreign jurisdiction while using standard cloud technologies.

### Summary

Sovereign regions are used for compliance and legal purposes. You need to use one if you deal with highly sensitive data, if its related to national security or financial complience or a specific requirement that your stored data must stay withing your countrys legal control.
