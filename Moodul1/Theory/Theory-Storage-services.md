# Module: Azure storage services
# Key notes

To actually build anything serious in Azure you gotta treat storage like the backbone of your whole setup (I know, not many do have a backbone, but to prevail here you have to have one). Blobs, files, tables, disks, redundancy levels it all decides whether your data survives a bad day or dies crying in a burning datacenter somewhere. Every choice you make here affects durability, performance, costs, and how smooth your apps run when real traffic hits.

**Bottom line:**
Understand your storage game now, cuz Future You does not wanna be that guy trying to recover corrupted blobs at 3 AM wondering why the whole app went down over a bad redundancy choice.


## Table of Contents
- [Azure storage accounts](#azure-storage-accounts)
- [Azure storage redundancy](#azure-storage-redundancy)
- [Azure storage services](#azure-storage-services)
- [Identify Azure data migration](#identify-azure-data-migration-options)
- [Identify Azure file movement](#identify-azure-file-movement-options)
- [Summary](#summary)
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

## Identify Azure data migration options

**Key points**
* Azure offers tools for moving servers apps and data from on premises to Azure  
* Two main paths  
  * **Azure Migrate** → online real time migration for servers apps databases  
  * **Azure Data Box** → offline bulk transfer using a physical device  

## Azure Migrate

**Key points**
* Central hub for assessing and migrating on prem workloads  
* Supports VMware Hyper V physical servers databases and web apps  
* Single portal to track everything end to end  

**Integrated tools**
1. **Discovery and Assessment**  
* Finds and analyzes on prem servers  
* Estimates readiness cost and sizing for Azure  

2. **Server Migration**  
* Moves VMware VMs Hyper V VMs physical servers and cloud VMs into Azure  

3. **Data Migration Assistant (DMA)**  
* Evaluates SQL Server compatibility  
* Flags unsupported features  
* Suggests migration path  

4. **Azure Database Migration Service**  
* Migrates SQL Server to  
  * SQL on Azure VM  
  * Azure SQL Database  
  * SQL Managed Instance  

5. **App Service Migration Assistant**  
* Assesses web apps  
* Migrates .NET and PHP sites to Azure App Service  

**Takeaway**

Azure Migrate = full stack migration hub for servers apps and databases

## Azure Data Box

**Key points**
* Physical device for offline bulk data transfer  
* 80 TB usable capacity  
* Rugged encrypted shipped directly to your datacenter  
* You copy data locally → send it back → Microsoft uploads it to Azure  

**When to use Data Box**
* Data size **over 40 TB**  
* No or slow network connectivity  
* One time or periodic bulk transfers  

**Import scenarios**
* Large one time migrations  
* Moving tape libraries to cloud  
* Migrating VMs SQL servers and apps  
* Moving historical data for analytics  
* Initial seed upload then incremental syncs over network  
* Periodic heavy data generation  

**Export scenarios**
* Disaster recovery  
* Legal or security requirements  
* Moving workloads back on prem or to another cloud  

**Security**
* Device is encrypted  
* Disks wiped after each job using NIST 800 88r1 standards  

**Takeaway**

Data Box = move massive data sets fast when the network cant handle it

## Identify Azure file movement options

**Key points**
* These tools are for moving **individual files or small groups of files**  
* Perfect when you dont need full datacenter migration  
* Main tools  
  * AzCopy  
  * Azure Storage Explorer  
  * Azure File Sync  

## AzCopy

**Key points**
* Command line tool for moving files and blobs to and from Azure Storage  
* Supports upload download copy and sync operations  
* Can transfer data between different storage accounts or even other cloud providers  
* Uses one direction synchronization only  
  (source → destination no two way sync)

**Use cases**
* Fast uploads to blob or file shares  
* Automated file transfers  
* Script based movement between accounts  
* Cloud to cloud file migration  

**Takeaway**

AzCopy = fastest low level tool for moving files in and out of Azure Storage

## Azure Storage Explorer

**Key points**
* Standalone graphical tool for managing Azure Storage  
* Works on Windows macOS and Linux  
* Uses AzCopy under the hood for performance  
* Lets you upload download delete and organize blobs and files visually  
* Supports cross account file movement  

**Use cases**
* When you want drag and drop instead of CLI  
* Managing many containers or blobs  
* Browsing storage accounts during development  

**Takeaway**

Storage Explorer = AzCopy power with a clean GUI for everyday storage management

## Azure File Sync

**Key points**
* Syncs your on prem Windows file servers with Azure Files  
* Turns a local file server into a cloud backed cache  
* Supports SMB NFS FTPS — any protocol Windows Server supports  
* Bi directional sync (unlike AzCopy)  
* Supports multiple file server caches across the world  

**Key features**
* Recover from hardware failure by reinstalling File Sync  
* Cloud tiering  
  * Hot files cached locally  
  * Cold files stored in Azure until needed  
* Centralizes storage in Azure without losing Windows Server compatibility  

**Use cases**
* Hybrid file sharing  
* Global office file access  
* Replacing or augmenting on prem file servers  
* Reducing local storage footprint  

**Takeaway**

Azure File Sync = hybrid file server done right local speed with cloud scale

## Summary

**Azure storage accounts**
* Your global namespace for all storage in Azure  
* Pick the right account type based on performance and workload  
* Naming matters because it defines your public endpoints  

**Redundancy**
* LRS protects against hardware failure  
* ZRS protects across zones  
* GRS and GZRS protect across regions  
* RA versions add global read options  

**Storage services**
* Blobs = unstructured massive scale  
* Files = cloud SMB and NFS shares  
* Queues = async message handling  
* Disks = block storage for VMs  
* Tables = lightweight NoSQL  

**Data migration**
* Azure Migrate handles real time server and app migration  
* Data Box handles massive offline bulk transfers  

**File movement**
* AzCopy for fast CLI transfers  
* Storage Explorer for GUI based file ops  
* File Sync turns on prem file servers into cloud backed caches  

**Takeaway**

Azure storage is not one service but a full toolbox.  
Choosing the right account type, redundancy model, and movement tool decides your performance, cost, durability, and how clean your architecture stays in the long run.
