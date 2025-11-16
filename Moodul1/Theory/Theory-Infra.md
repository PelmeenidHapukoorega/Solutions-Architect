# Module: Describe the core architectural components of Azure
# Key notes

In order to design anything you have to understand how regions, subs and resources all fit together like its a 25 piece puzzle. This foundation determines how stable, secure and scalable your solutions will end up.
**Bottom line** Architect brain now so future you doesnt have to clean up dumb shit later.

## Table of Contents
- [Azure Regions](#azure-regions)
- [Region Pairs](#region-pairs)
- [Sovereign Regions](#sovereign-regions)
- [Availability Zones](#availability-zones)
- [AzureDatacenters](#azure-datacenters)
- [Azure Resources and Resource groups](#azure-resources-and-resource-groups)
- [Subscriptions](#subscriptions)
- [Management Groups](#management-groups)
- [Hierarchy of Resources](#hierarchy-of-resources)


# Azure regions

* Geographical areas that contain at least 1 if not more datacentres that are nearby each other and networked together with low latency network.
* Azure assigns and controls resources within each region to ensure balanced workloads.

Note! Some services or VM features are only available in certain regions: VM sizes or storage types.
```mathematica
Azure Region

 ├── AZ 1
 
 ├── AZ 2
 
 └── AZ 3
 ```

# Region pairs

**Key Takeaways**
* Most regions are paired with other regions for further resiliency.
* It allows for the replication of resources across geography, helps reduce the likelihood of interruptions in case of natural disasters, civil unrest, power outages or meteor shower that killed the dinosaurs: point being, it survives.
* Not all Azure services automaticallyt replicate data or fall back from failed region to cross replicate to another region. In this case recovery and replication must be confgured by customer aka user.

# Additional advantages

**Key Takeaways**
* During an extensive outage one region ot of every pair is prioritized to make sure at least one is restored ASAP for apps hosted in that region pair.
* Updates are rolled out a pair at a time to minimize downtime and risk outage.
* Data continues to reside within the same geography for tax and law purposes.
* Regions paired in two directions allow being each others backup, however West india and Brazil south regions are one direction. In one direction pairing the primary region does not backup its secondary.


# **Sovereign regions**

**Key Takeaways**
* Instances isolated from the main instance of Azure.
* Mostly used for compliance or legal purposes.
  
# **Availability zones**

**Key Takeaways**
* Physically seperate datacentres within a region.
* Each zone is self sufficient (independent power, cooling and networking).
* Set up as isolation boundary. If one goes down the other continues.
* Each region has a minimum of 3 seperate AV zones, however not all regions support AZ zones.

# **Azure datacenters**

**Key Takeaways**
* Large datacentres that are part of the physical infrastructure of Azure. Has everything that large corporate datacentres have.
* Are not directly accessible.
* Are grouped into: Azure regions or Azure availability Zones.

# Azure Resources and Resource groups.

**Key Takeaways**
* **Resource** is a basic building block, basically anything you create is a resource (VM, databases, etc).
* **Resource group** is a group that contains all created resources.
* Resource groups cant be nested.
* Single resource can only be in one group at a time.
* Moving resources between groups will no longer be associated with the former group.
* Convenient to group everything together.
* Deleting the group will delete everything in it.
* When provisioning resources, think about resource group structure that best suit the needs.
* Its best to group resources based on access schema and then assign access at the resource group level
* Maximize their usefulness.

# **Subscriptions**

**Key Takeaways**
* Unit of management, billing and scale.
* A way to logically organize resource groups and facilitate billing (think resource groups).
* Account can have multiple subs.
* In a multi sub account you can use subs to configure billing models and apply different access management policies.
* You can define boundaries: Billing boundary and Access control boundary.
* Billing boundary determines how Azure account is billed for using it. it can create multiple subs for different tyos of billing req. Azure generates billing reports and invoices for each sub so you can learn some organizing skills and manage costs. Think a mix between tetris and monopoly.
* Access control boundary applies access management policies at the sub level, you can create seperate subs to reflect organizational structures. Think different deparments, each having their own subscription policies. Allows you to manage and control access to resources that users provision with specific subs.
* When you first sign up to Azure you get a free trial subscription that lets you start exploring. Once you feel confident enough and want to deep dive further what Azure has to offer then you can get additional subscriptions at extra cost.
* Free account gives you access to 25 products that are always free.
* Credit use for the first 30 days (200 Eur).
* Free access to most popular Azure products for 12 months.

# **Management groups**

**Key Takeaways**
* Enterprise level management at a larger scale no matter the subscription type.
* Can be nested.
* Efficient way to manage access, policies and compliance for those subscriptions.

# **Hierarchy of resources**
* You can build flexibile structure of management groups and subs to organize resources into hierarchy.
* Unified policy and access management.
* Examples of use: Hierarchy creation that applies a policy, provide user access to multiple subs.

```pgsql
Management Group
      ↓
  Subscription
      ↓
 Resource Group
      ↓
     VM
```


# **Important!** 
* 10 000 management groups can be supported in a single directiory.
* Management group tree can support up to six levels of depth. The limit doesnt include the root level of the sub.
* Each group and subscription can support only 1 parent.
















