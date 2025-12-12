# Identify Azure data migration options

## Table of contents
* [Azure migrate](#azure-migrate)
* [Integrated tools in depth](#integrated-tools-in-depth)
* [Azure data box](#azure-data-box)

## My interpretations

## Azure migrate

### Why does it exist?

To solve complex challenges when it comes to moving existing workloads (servers, applications, databases and virtual desktops)
from on prem to cloud environment.

### What does it do?

It acts as a single console to help an organization discover, assess and migrate its existing workloads to Azure.

### How does it work?

It works as a structured 3 phase process to move your on prem to Azure cloud with minimal risk and optimized costs.
You start by deploying appliance onto physical server, the appliance then performs agentless scan to collect configuration 
metadata, then Azure analyses collected data to perform assessment. In phase 2 you set up replication, the tool then performs initial
full replication of your source servers disks and data to Azure storage. Afterwards you need to run a non disruptive test migration,
it creates a fully functional copy of your server in Azure but keeps on prem running, this validates that app works correctly before
finalizing the move. Phase 3, after successful testing you execute the final planned cutover, post migration you should continously
monitor the migrated workloads to ensure performance meets benchmarks and to look for further cost reduction opportunities.

### Provides following

* Unified migration platform: Single portal to start, run and track migration to Azure
* Range of tools: For assessment and migration. Migrate tools include Azure migrate: Discovery and assessment and Azure migrate: server migration.
* Migrate also integrates with other services and tools within Azure as wells as independent sofware vendor (ISV) offerings.

### Summary

Azure migrate is for organizations wanting to migrate their infra to cloud with minimal costs and risks. I.e its a low risk path to 
the world of cloud.

## Integrated tools in depth

* Azure Migrate: Discovery and assessment
  * To asses on prem servers running on VMware, hyper V and physical server in pre for migration to cloud.
* Azure Migrate: Server migration
  * Migration of VMware VMs, other VMS, physical servers and others to cloud.
* Data migration assistant
  * Stand alone tool to assess SQL servers. Helps pinpoint potential problems blocking migration.
  * Identifies unsupported features, new features that can benefit you after migration and the right path for database migration.
* Azure database migration service.
  * Migrate on prem databases to Azure VMs running SQL server, Azure SQL Database or SQL managed instances.
* Azure app service migration assistant
  * Standlone tool to assess on prem websites for migration to Azure app service.
  * Use it to migrate .NET and PHP web apps to Azure.
* Azure data box
  * Use it to move large amounts of offline data to Azure.

## Azure data box

Azure data box is a physical migration service that helps transfer large amounts of data in a quick, inexpensive and reliable way.

### Why does it exist?

To solve fundamental problem of moving massive amounts of data into or out of Azure when network transfer is impractical, too slow or even too expensive. It accelerates large scale data transfer by shifting the heavy lifting from your internet connection to physical shipping.

### What does it do?

Its a physical storage appliance that moves terabytes or petabytes of data into and out of Azure in a quick, secure and cost effective manner.

### How does it work?

It works through standardized, secure ship to load process, which in essence is managed end to end via Azure portal. Steps may vary depending on whether you are importing or exporting.

### Summary

Azure data box is a service primarily used to move large amounts of data from on prem to  Azure cloud or from  Azure cloud to on prem, it works in both directions. 

### Use cases

* Onetime migration: When a large amount of on prem data is move to Azure.
* Moving a media library from offline tapes into Azure to create an online media library.
* Migrating your VM farm, SQL server and apps to Azure.
* Moving historical data to Azure for in depth analysis and reporting using HDInsight.
* Initial bulk transfer: when initial bulk transfer is done using Data Box (seed) followed by incremental transfers over the network.
* Periodic uploads: when large amount of data is generated periodically and needs to be moved to Azurre.

### Scenarios where Data box can be used to export data from Azure

* Disaster recovery
* Security requirements
* Migrating back to on prem or to another cloud service provider.

**Important!**

Once the data from your import order is uploaded to Azure, the disks on the device are wiped clean in accordance with NIST 800-88r1 standards. For export the disks are erased once the device reaches Azure data center.
