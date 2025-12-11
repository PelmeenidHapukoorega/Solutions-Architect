# Describe Azure storage services

## Table of contents
* [Benefits of Azure storage](#benefits-of-azure-storage)
* [Azure blobs](#azure-blobs)
* [Blob storage tiers](#blob-storage-tiers)
* [Azure files](#azure-files)
* [Azure queues](#azure-queues)
* [Azure disks](#azure-disks)
* [Azure tables](#azure-tables)

## My interprentations

Storage platform includes following services:

* Azure blobs: Massively scalable object store for text and binary data. Also supports big data analytics through Data Lake Storage Gen2.
* Azure files: Managed file shares for cloud or on prem deployments.
* Azure queues: Messaging store for reliable messaging between application components.
* Azure disks: Block level storage volumes for Azure VMs.
* Azure tables: NoSQL table option for structured, non relational data.

## Benefits of Azure storage

* Durable and highly available.
  * Redundancy ensures that data is safe if hardware failures occur. You can replicate data across data centers or geo regions for extra protection.
* Secure.
  * All data written to a storage account is encrypted by the service. Azure storage provides you with fine gained control over who has access to your data.
* Scalable.
  * Its designed to be massively scalable to meet the data storage and performance needs of todays applications.
* Managed.
  * Azure handles hardware maintenance, updates and critical issues for you.
* Accessible.
  * Data is accessible from anywhere in the world over HTTP or HTTPS. Supports variety of languages as well as mature REST API. Supports scripting in powershell or CLI.


## Azure blobs

### Why does it exist?

They are essential cloud storage option for massive amounts of unstructured data that cannot be easily organized into traditional databases or file systems. 

### What does it do?

Core function is to be massively scalable, secure and durable cloud storage for unstructured data.

### How does it work?

It works by organizing your data into a simple hierarchical structure and providing different blob types optimized for various tasks.

### Summary

Storage blobs are essentially the basic storage option, its a good pick if you have a lot of unstructured data that you dont use often but dont want to store it in other storage options because it might get expensive. Blobs are perfect to store mass amounts of data that you might use 1 or 2 a year and it keeps the cost to a minimum aka Archive Tier.

### Ideal for

* Serving images or docs directly to browser.
* Storing files for distributed access.
* Streaming videos and audio.
* Storing data for backup and restore, disaster recovery and archiving.
* Storing data for analysis by an on prem or azure hosted service.

## Blob storage tiers

### Why does it exist?

Exists entirely for cost optimization by matching storage price to the access frequency and retrieval speed required by your data.

### What does it do?

To allow you optimize costs by aligning thre price you pay with how often you need to access your data.

### How does it work?

It uses policy based on rules. Storage lifecycle management is governed by a policy that consists of 1 or more rules that you define for your storage account.

### Summary

Blob storage tiers exist so you have multiple options to pick from while considering your business needs, cost and the efficieny/ reliability you need for your apps.

### Tiers

* Hot access tier: Optimized for storing data that is accessed frequently (images/ videos).
* Cool access tier: Optimized for data that is infrequently accessed and stored for at least 30 days (invoices for customers).
* Cold access tier: Optimized for storing data that is infrequently accessed and stored for at least 90 days.
* Archive access tier: Appropriate for data that is rarely accessed and stored for at least 180 days, with flexible latency requirements (long term backups).

Following considerations that apply to different access tiers:

* Hot, cool and cold access tier can be set at the account level. Archive access tier isnt available at account level.
* Hot, cool, cold and archive tiers can be set at the blob level, during or after upload.
* Data in the cool and cold tiers can tolerate slightly lower availability, but still requires high durability, retrieval latency and throughput characteristics similar to hot data. Cool and col data lower availability SLA and higher access costs compared to hot data are acceptable trade offs for lower storage costs.
* Archive storage stores data offline and offers the lowest storage costs, but alos the highest costs to rehydrate and access data.

## Azure files

### Why does it exist?

To solve a problem of sharing files using familiar, industry standard file protocols making it ideal choice for lift and shift migration scenarios since blobs cannot replace traditional network file server.

### What does it do?

It functions as a traditional network file server in the cloud. Its designed to replicate the functionality of windows or linux file share, making it easy to migrate existing apps and share files across various deployments.

### How does it work?

By creating a fully managed file share service and exposing it over standard network protocols that traditional OS systems are already built to use.

### Summary

Basically files is like this shared folder on the internet that everyone in your "digital" family can use as a unified library for files.

### Benefits of Azure files

* Shared access: Supports industry standard SMB and NFS protocols, meaning you can seamlessly replace on prem file shares with Azure files without worrying about compatibility.
* Fully managed: No need to manage hardware or OS.
* Scripting and tooling: You can use CLI and powershell to create, mount and manage file shares as part of the administration of Azure apps.
* Resiliency: Replacing on prem file shares with Azure files means you dont have to deal with local power outages or network issues.
* Familiar programmability: Apps running on Azure can access data in the share via file systems I/O APIs. Devs can therefore use their existing code and skills to migrate existing apps. 

## Azure queues

### Why does it exist?

To enable decoupled, asynchronous communication between different parts of an app or different services.

### What does it do?

Acts as a simple reliable buffer to manage tasks and increase resilience of cloud native apps.

### How does it work?

It works by using simple first in, first out aka FIFO process to manage tasks: Sending service (producer) puts a message at the end of queue. Receiving (User), working service continously checks the front of the queue and pulls a message, which then becomes invisible to others for a time (visibility timeout). If the workes finishes task, it deletes the message, if it fails or crashes, the visibility timeout expires and the message automatically reappears for another worker to process.

### Summary

Basically its the bouncer in front of the club waiting til some people leave the club in order to let more in. In other words its to ease the workload and make.

## Azure disks

### Why does it exist?

Because cloud VMs need a reliable high performance and persistent place to store their OS, apps and data just like a physical computer needs an SSD or HDD.

### What does it do?

Serves as a persistent Virtual HDD for VMs. Its job is to ensure that you VMs OS, apps and data remain safely stored and available even if the VM is shut down, restarted or moved to a different physical server.

### How does it work?

It works by treating VHD as a specialized blob that is fully managed by Azure and designed for continous attachment for VMs.

### Summary

Azure disks is storage option exclusively for VMs. Its like HDD or SSD is to a physical computer.

## Azure tables

### Why does it exist?

To provide simple, low cost and massively scalable NoSQL database solution for storing structured but non relational data in the cloud.

### What does it do?

It functions as a highly scalable, simple and low cost NoSQL data store for structured non relational data. Its designed to handle massive volumes of data where speed and cost effectiveness are more important than complex database features (like SQL joins or transactions)

### How does it work?

It works as asimple key value store that achieves massive scalability by using a specialized 2 part key to organize and distribute your data across many storage nodes.

### Summary

Tables is like a huge spreadsheet where you can store huge lists of things. It keeps your list organized and it doesnt care if every row has different info. Since its so simple it can hold millions of lists without slowing down, making it the cheapest way to manage very large collections of simple data.


