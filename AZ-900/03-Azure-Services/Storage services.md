# Describe Azure storage services

## Table of contents
* [What is Shared Responsibility Model?](#what-is-shared-responsibility-model)
* [Cloud Models](#cloud-models)
* [Azure Arc](#azure-arc)
* [Azure VMware](#azure-vmware)
* [Consumption Model](#consumption-model)

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




