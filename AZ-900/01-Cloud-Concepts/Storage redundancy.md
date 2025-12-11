# Describe Azure Storage redundancy

## Table of contents
* [Redundancy in the primary region](#redundancy-in-the-primary-region)
* [Locally redundant storage](#locally-redundant-storage)
* [Zone redundant storage](#zone-redundant-storage)
* [Redundancy in a secondary region](#redundancy-in-a-secondary-region)
* [Geo redundant storage](#geo-redundant-storage)
* [Geo zone redundant storage](#geo-zone-redundant-storage)
* [Read access to data in 2ndary region](#read-access-to-data-in-2ndary-region)

## My interpretations

Azure storage always stores multiple copies of your data so that its protected from planned and unplanned events such as transient hardware failures, network or power outages and natural disasters. Redundancy ensures that your storage account meets its availability and durability targets even in the face of failures.

### Why does it exist?

To guarantee that your data will not be lost and that your application will always be able to access it, even when faced with inevitable failures.

### What does it do?

Storage account redundancy options allow you to balance 3 critical trade offs based on your application needs: Cost, latency and resilience/durability.

### How does it work?

Storage redundancy ensures that if one component fails another is immediately ready to take over, keeping the data available and consistend, regardless of the scale of the failure.

### Summary

Redundancy exists overall to ensure that there is minimal or no downtime for your application services. Different types of storage accounts offer different solutions based on your needs. Its a failsafe for your data.

### Factors in choosing redundancy options you should consider include:

* How your data is replicated in the primary region. Why? The choice between LRS or ZRS determines your resilience against failures within a single physical location.

* Whether you data is replicated to a 2nd region that is geographically distant to the primary region, to protect against regional disasters. Why? The choice (GRS/GZRS vs LRS/ZRS) determines your ability to peform disaster recovery.

* Whether your application requires read access to the replicated data in the secondary region if the primary region becomes unavailable. Why? The choice (RA-GRS vs GRS/GZRS) determines your recovery time objective (RTO) and allows for higher availability and better performance.

## Redundancy in the primary region

* Data in storage account is always replicated 3 times in primary region.
* Storage offers 2 options for how data is replicated: Locally redundant LRS or zone redundant ZRS

## Locally redundant storage

* LRS replicates data 3 times within a single data center in the primary region.
* LRS provides at least 11 nines of durability of objects over a given year.
* Lowest cost redundancy option.
* Least durable.
* Protects agains server rack and drive failures.
* If disaster hits a data center all replicas of storage account may be lost or unrecovarable.

**PS!:** Microsoft recommends using ZRS, GRS or GZRS to mitigate risk of losing data.

## Zone redundant storage

* For Availability zone enabled regions.
* ZRS replicates storate data synchronously across 3 AZ zones in the primary region.
* ZRS provides durability of 12 ninens over a given year.
* With ZRS data is still accessible for both read and write operations, even if a zone becomes available.'
* No remounting of Azure file shares from the connected clients is required.
* If zone becomes unavailable, Azure undertakes networking updates such as DNS repointing.
* Updates may affect your app if you access data before updates have completed.

**PS:** ZRS is recommended in primary region for high availability. ZRS is also recommended for restricting replication of data withing a country or region to meet data governance requirements.

## Redundancy in a secondary region

For apps requiring high durability, you can choose to additionally copy the data in your storage account to a 2ndary region that is hundreds of kilometres away from the primary region. 

If the data in your storage account is copied to a 2ndary region, then your data is durable even in catasthrophic events that prevent primary region from being recovered.

When creating storage account you select the primary region for the account. Paired 2ndary region is based on region pairs and cant be changed.

2 options for copying your data to 2ndary region:

* GRS aka geo redundant storage.
* GZRS aka geo zone redundant storage.

### Need to know

* By default, data in 2ndary region isnt available for read or write access unless there is a failover to the 2ndary region.
* If primary region becomes unavailable, you can choose to fail over to the 2ndary region.
* After failover the 2ndary region will become the primary region and you can read and write data.

**Important!**

Because data is replicated to the 2ndary region asynchronously, failure that affects primary region may result in data loss if the primary region cant be recovered.

## Geo redundant storage

* GRS copies data synchronously 3 times within a single physical location in the primary region using LRS.
* Then it copies your data asynchronously to a single physical location within 2ndary region using LRS.
* GRS provides durability of 16 nines over a given year.

## Geo zone redundant storage

* GZRS combines high availability provided by redundancy across AZ zones with protection from regional outages by geo replication.
* Data in GZRS is copied across 3 AZ zones in the primary region, and is also replicated in the 2ndary geo region using LRS.
* GZRS provides durability of at least 16 nines over a given year.

**PS!** Microsoft recommends using GZRS for apss requiring maximum consistency, durability and availability, excellent performance and resilience for disaster recover.

## Read access to data in 2ndary region

GRS or GZRS replicates your data to another physical location in the 2ndary region to protect agains regional outages. However the data is available to be read only if the user or Microsoft initiates a failover from the primary region to 2ndary region. If you enable read access to 2ndary region your data is always available, even when primary region is running optimally. For read access to the 2ndary region, enable RA-GRS or RA-GZRS.

**Important!**

Your data in 2ndary region may not be up to date due to RPO.
