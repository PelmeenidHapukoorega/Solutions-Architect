# Describe Azure Storage accounts

## Table of contents
* [Redundancy options](#redundancy-options)
* [Locally redundant storage (LRS)](#locally-redundant-storage-lrs)
* [Geo redundant storage (GRS)](#geo-redundant-storage-grs)
* [Read access geo redundant storage (RA-GRS)](#read-access-geo-redundant-storage-ra-grs)
* [Zone redundant storage (ZRS)](#zone-redundant-storage-zrs)
* [Geo zone redundant storage (GZRS)](#geo-zone-redundant-storage-gzrs)
* [Read access geo zone redundant storage (RA-GZRS)](#read-access-geo-zone-redundant-storage-ra-gzrs)
* [For future reference](#for-future-reference)

## My interpretations

Storage accounts are a foundational building blocks for storing your data. They provide a unique namespace for Azure storage data objects, making your data accessible from anywhere in the world over HTTP or HTTPS.

### Why does it exist?

To solve the fundamental problems of data management in the cloud. Storage accounts are a necessary abstraction layer (meaning not worrying about infra) that allows cloud providers to deliver robust, scalable and cost efficient storage services.

### What does it do?

Performs several critical continuous functions to manage and protect your data. It doesnt just hold it, it actively manages access, scale and resilience.

### How does it work?

It works by providing a highly secure, scalable and resilient software defined layer over raw physical disk space. It uses unique namespace to address data and automated replication engine to ensure data is never lost.

### Summary

Storage accounts is where you hold your data. In azure all the data stored in storage accounts gets a unique namespace to better disginuish your files, it has automated replication engine which ensures the durability and high availability of your data which means its never lost. Think of storage accounts like loot chests in RPG videogames, its where you either get good loot or you store some.

## Redundancy options

## Locally redundant storage (LRS)

### Why does it exist?

Its low cost, low latency option for data protection. Its the base level of data protection that everyone gets automatically, even if you decided to upgrade to a higher redundancy later.

### What does it do?

Its the fundamental method in Azure and its primary job is to deliver low cost, fast performance and protection against hardware failures. It Synchronously creates 3 copies. Distributes copies within a single data center. Provides durability of 11 nines. Ensures lowest latency and satisfies data residency requirements.

### How does it work?

It works through a process of synchronous replication that happens entirely within 1 physical data center. In short, client initiates write request which triggers synchronous replication. Then it writes the data to 3 replicas simultaneously, then you wait for success confirmation. It all works relatively quickly due to low latency connection. If there is a failure it has automatic failover in place, if one replica fails, the storage account instantly starts serving read requests from one of the other healthy replicas. Then the systems starts repairing the failed copy in the background.

### Summary
LRS is the cheapest storage option that gives you fast performance and cost efficiency. Its a simple way to protect and store data, but since its redundancy is local it means that if a region wide disaster should accour then you can say goodbye to your data.

## Geo redundant storage (GRS)

### Why does it exist?

To provide disaster recovery capability and the highest level of data durability against catastrophic, regional events (meteor shower).

### What does it do?

It is designed to ensure maximum data durability and business continuity agains large scale catastrophic failures that affect an entire geographical region.

### How does it work?

It works by combining local, synchronous data protection with wide are, asyhchronous replication to a physically distant region. This dual approach ensures high speed and protection against localized failure in the primary region, while providing ultimate protection against catastrophic regional failure.

### Summary

GRS is a storage option in case of a regional failure. If a data center is affected it will replicate all your data in another data center in another distant region to ensure your data isnt lost, essentially it back ups your data in another data center thats around 300 smt KM away.

## Read access geo redundant storage (RA-GRS)

### Why does it exist?

To solve key limitation of standard GRS storage option and provide the highest possible availability for read operations in a mission critical applications. In standard GRS the data in the secondary region is inaccessile for both read and write options unless you manually trigger a full account failover aka disruptive process.

### What does it do?

It does everything that GRS does but adds function of making the disaster recovery copy accessible for reading thus massively improving application resilience and availability.

### How does it work?

It works by executing the same high durability and 6 copy replication as GRS but with added step of exposing 2nd, seperate endpoint for reading data from the distant secondary region.

### Summary

RA-GRS is a top tier choice for apps where continuous data accessibility for reading is paramount even if sacrificing lowest write latency. In short it automatically copies your data to a distant region and makes the distant copy available for reading at all times. This ensures your application can still access data even if the primary region is temporarily available.

## Zone redundant storage (ZRS)

### Why does it exist?

To provide mid level protection that covers agains the failure of an entire data center or availability zone within a single region, while still maintaining low latency and high performance. Aka it fills the gap between LRS and GRS, a middle ground.

### WHat does it do?

Its designed to provide high durability and high availability within a single azure region, by distributing data copies across different physical locations.

### How does it work?

It works by using synchronous replication to secure 3 copies of your data across 3 geographically seperate yet locally connected, physical locations within the same region. Client initates request, azure starts synchronous cross zone write, high availability is provided through isolation and since all three AZ zones are interconnected within the same area using low latency network, means the process is fast, minimizing the performance impact on your app.


### Summary

ZRS offers high availability within a single region that also protects agains data center wide failures, while still maintaining high performance. Its a good choice if availability within primary region is non negotiable and if the workload is performance sensitive.

## Geo zone redundant storage (GZRS)

### Why does it exist?

To provide absolute maximum level of data resilience and availability by combining best features of both ZRS and GRS. It is designed for the most demanding, mission critical apps that cannot afford any downtime from either a local disaster or region wide catastrophe, think goverment level. 

### What does it do?

It combines the benefits of ZRS and GRS. It essentially performs a 3 part function to ensure maximum data protection and availability. Provides high availability by replicating your data 3 times across 3 seperate isolated AZ zones withing primary region. It asynchronously copies your data to a distant secondary region. Maintains 3 replicated copies in the primary regions AZ zone and 3 replicated copies in the distant secondary region.

### How does it work?

It provides high availability by replicating your data 3 times across 3 seperate isolated AZ zones withing primary region. It asynchronously copies your data to a distant secondary region. Maintains 3 replicated copies in the primary regions AZ zone and 3 replicated copies in the distant secondary region.

### Summary

GZRS is the top choice for storage account. It offers 16 nines of durability and protection agains pretty much anything. Its like owning 3 seperate cars that you daily drive and have a big collection of cars big garage in another town.

## Read access geo zone redundant storage (RA-GZRS)

### Why does it exist?

To provide the absolute highest level of resilience and accessibility available in Azure storage, essentially eliminating single points of failure for both regional disasters and temporary zone level outages, while still ensuring read access is always maintained. Its the best choice for the most critical and globally distributed applications.

### What does it do?

Its designed to perform every protection function available, ensuring both maximum durability (16 nines) and maximum read availability.

### How does it work?

It provides ultimate local resilience by synchronously replicating your data across 3 seperate AZ zones in the primary region. Then it asynchronously replicates the data to a single, secure, geographically distant secondary region. It offers continous read accessibility, by providing a seperate, dedicated read only endpoint for the secondary region. In total it will maintain 6 copies of your data.

### Summary

In short the RA-GZRS exists to do 1 thing, to enusre your mission critical data is protected agains every possible infra failure (local, zone or regional) and remains available for reading under virtually all circumstances.

## For future reference:

<img width="912" height="447" alt="image" src="https://github.com/user-attachments/assets/9e5a2653-5bce-45ff-b47e-2d398944a24c" />

### Storage account endpoints

One of the benefits of Azure storage account is having unique namespace in Azure for your data. In order to do this, every storage acc in Azure must have a unique in Azure account name. The combination of account name and the storage service endpoint forms the endpoints for your storage account.

### When naming

* Must be between 3 and 24 characters in length, may contain numbers and lowercase only.
* Must be unique within Azure.

Example table:

<img width="915" height="317" alt="image" src="https://github.com/user-attachments/assets/cfca1b9e-5b4b-4e72-adc7-0ae3fcc4da08" />

