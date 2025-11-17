# Module: Azure storage services
# Key notes

To actually design anything in Azure you gotta understand the compute and networking stack like its your own toolbox. VMs, functions, containers, VNets, subnets, routing they all snap together like a puzzle that decides how fast, secure and reliable your whole setup runs.

**Bottom line:**
Build smart now, because Future You aint trying to debug a busted network at 2 AM wondering why a VM cant talk to anything.


## Table of Contents
- [Azure storage accounts](#azure-storage-accounts)
- [Azure storage redundancy](#azure-storage-redundancy)
- [Azure storage services](#azure-storage-services)
- [Identify Azure data migration](#identify-azure-data-migration)
- [Identify Azure file movement](#identify-azure-file-movement)
#

## Azure storage accounts

**Key points**
* Storage account gives you a unique namespace for your data reachable anywhere through HTTP or HTTPS
* Data stays secure highly available durable and built for massive scale
* Account type decides what services you get and what redundancy options you can use
* You always start by choosing the account type based on your scenario

**Storage account types**

1. **Standard general purpose v2**
* Supports blobs queues tables Azure Files and Data Lake Storage
* Redundancy options include LRS, GRS, RA GRS, ZRS, GZRS, RA GZRS
* Default choice for most workloads
* Use this for everyday storage unless you need premium performance or special protocols

2. **Premium block blobs**
* High performance storage for block blobs and append blobs
* Supports LRS and ZRS
* Best when you need low latency and high transaction rates or deal with lots of small objects

3. **Premium file shares**
* Designed for Azure Files only
* Supports SMB and NFS in the same account
* Use for enterprise level file shares high performance apps or large scale file workloads

4. **Premium page blobs**
* Only for page blobs
* Supports LRS
* Used mainly for virtual machine disks that need consistent low latency

**Storage account endpoints**
* Every storage account needs a unique name across Azure
* Name must be between 3 and 24 characters and use lowercase letters and numbers
* Name combined with the service endpoint creates the final access URL

**Endpoint formats**
Blob Storage  
`https://<storage-account-name>.blob.core.windows.net`

Data Lake Storage Gen2  
`https://<storage-account-name>.dfs.core.windows.net`

Azure Files  
`https://<storage-account-name>.file.core.windows.net`

Queue Storage  
`https://<storage-account-name>.queue.core.windows.net`

Table Storage  
`https://<storage-account-name>.table.core.windows.net`

**Takeaway**

Storage account = master container in Azure.  
You pick the type based on performance durability and what services you plan to use.  
Naming matters because it creates your public facing endpoints.  
Choose right now so you do not refactor pain later.

## Azure storage redundancy

**Key points**
* Azure stores multiple copies of your data to protect against hardware failure, network issues, power loss, zone outages, and regional disasters
* Redundancy keeps your data available, durable, and recoverable even when something fails
* You choose redundancy based on cost, availability needs, and disaster recovery requirements
* You also choose whether you need read access in the secondary region

**Redundancy in the primary region**
Azure always stores at least three copies of your data in the primary region  
You select either LRS or ZRS for how those copies are placed

## Locally redundant storage (LRS)

**Key points**
* Stores three copies inside one datacenter
* Lowest cost redundancy option
* Eleven nines durability
* Protects against hardware failures inside the building
* Does not protect against total datacenter loss

**LRS diagram**
```yaml
Primary Region
+--------------------------------+
| Datacenter |
| Copy1 Copy2 Copy3 |
+--------------------------------+
```

**Takeaway**
LRS = protects you from local hardware failures but not from full building disasters

## Zone redundant storage (ZRS)

**Key points**
* Stores three synchronous copies across three availability zones
* Twelve nines durability
* Data stays available for read and write even if a full zone goes offline
* Ideal for high availability and data residency rules

**ZRS diagram**
```yaml
Primary Region
+---------+ +---------+ +---------+
| Zone 1 | | Zone 2 | | Zone 3 |
| Copy 1 | | Copy 2 | | Copy 3 |
+---------+ +---------+ +---------+
```

**Takeaway**
ZRS = protects against zone failure while keeping data fully available

## Redundancy in a secondary region

**Key points**
* Adds protection from region-wide disasters
* Uses Azure region pairs
* Data is copied to a distant secondary region
* You choose GRS or GZRS

## Geo-redundant storage (GRS)

**Key points**
* Primary region uses LRS
* Data is copied asynchronously to a secondary region using LRS
* Sixteen nines durability
* Secondary region cannot be read unless failover occurs

**GRS diagram**
```yaml
Primary Region (LRS)
Copy1 Copy2 Copy3
|
| async replication
v
Secondary Region (LRS)
Copy1 Copy2 Copy3
```

**Takeaway**
GRS = LRS in two regions for global disaster recovery

## Geo-zone-redundant storage (GZRS)

**Key points**
* Primary region uses ZRS
* Data is copied asynchronously to a secondary region using LRS
* Maximum durability, availability, and disaster protection
* Best for apps needing consistent performance and recovery guarantees

**GZRS diagram**
```yaml
Primary Region (ZRS)
Zone1 Copy1 Zone2 Copy2 Zone3 Copy3
|
| async replication
v
Secondary Region (LRS)
Copy1 Copy2 Copy3
```

**Takeaway**
GZRS = ZRS protection inside the region plus LRS protection in the paired region

## Read-access redundancy (RA GRS and RA GZRS)

**Key points**
* Secondary region is normally locked until failover occurs
* RA modes allow reading directly from the secondary region at all times
* Useful for global read workloads
* Secondary region may lag behind due to asynchronous replication (RPO delay)

**RA diagram**
```yaml
Primary Region ---> Secondary Region
Read Write Read Only
```

**Takeaway**
RA versions = give global read scaling, but secondary data may be slightly behind

## Azure storage services

**Key points**
* Azure Storage is a full platform with multiple data services built for different workloads  
* Highly durable highly available secure and globally accessible  
* Scales automatically to handle massive amounts of data  
* Azure handles hardware maintenance encryption and infrastructure so you dont have to  
* Access your data using REST API Azure CLI PowerShell SDKs or Azure Storage Explorer  

## Azure Blobs

**Key points**
* Massively scalable object storage for text binary data and big data workloads  
* Stores unstructured data without format restrictions  
* Handles thousands of uploads streaming massive videos fast growing logs and custom data formats  
* Doesnt require disk management Azure handles the backend  

**Blob use cases**
* Serve images or documents directly to browsers  
* Distributed access to shared data  
* Stream audio or video  
* Backup restore archiving and DR  
* Data lakes and analytics workloads  

**Blob access**
* Access via URL HTTP HTTPS REST API client libraries Azure CLI PowerShell  

**Blob storage tiers**
* Hot tier for frequently accessed data  
* Cool tier for infrequent access stored at least 30 days  
* Cold tier for infrequent access stored at least 90 days  
* Archive tier for rarely accessed long term data stored at least 180 days  

**Takeaway**
Blob storage = is your go to when you need scale flexibility and cost management for growing data  

## Azure Files

**Key points**
* Fully managed cloud file shares using SMB or NFS  
* Can be mounted by cloud and on prem machines at the same time  
* Supports Windows Linux macOS  
* Can be cached locally using Azure File Sync  

**Azure Files benefits**
* Shared access using industry standard file protocols  
* No server OS management or hardware headaches  
* Manage through CLI PowerShell portal or Storage Explorer  
* High resiliency built for always on file sharing  
* Applications can use normal file IO code without rewriting  

**Takeaway**
Azure Files = replaces traditional file servers with cloud scale shares you never have to patch  

## Azure Queues

**Key points**
* Message storage service for asynchronous communication  
* Stores millions of messages up to 64 KB each  
* Access worldwide over HTTPS with authentication  
* Ideal for background processing decoupled systems and event driven workflows  

**Queue use cases**
* Build work backlogs  
* Trigger Azure Functions when new messages arrive  
* Process web form submissions  
* Offload long running tasks from the main app  

**Takeaway**
Queues = keep your app responsive by letting background work run separately and safely  

## Azure Disks

**Key points**
* Managed block level storage for Azure VMs  
* Works like a physical disk but fully virtualized  
* Durable secure and highly available  
* Azure handles replication patching and hardware issues  

**Use cases**
* Operating system disks  
* Data disks for databases apps and VM workloads  
* High performance premium SSD and ultra disk scenarios  

**Takeaway**
Azure Disks give VM workloads fast durable block storage without physical hardware limits  

## Azure Tables

**Key points**
* NoSQL key value store for structured non relational data  
* Highly scalable and low cost  
* Accepts authenticated calls from anywhere hybrid or multi cloud  
* Perfect for large simple datasets  

**Use cases**
* Metadata storage  
* Logs device data customer data  
* Lightweight backend storage for apps  

**Takeaway**
Azure Tables = are ideal for huge structured datasets without needing a full database engine  

## Identify Azure data migration

## Identify Azure file movement
