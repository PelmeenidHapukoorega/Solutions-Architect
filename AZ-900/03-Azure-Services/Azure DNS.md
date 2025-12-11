# Describe Azure DNS

## Table of contents
* [Benefits of Azure DNS](#benefits-of-Azure-DNS)
* [Reliability and performance](#reliability-and-performance)
* [Security](#security)
* [Ease of use](#ease-of-use)
* [Customizable VNets with private domains](#customizable-vnets-with-private-domains)
* [Alias records](#alias-records)

## My interpretations

Azure DNS is a hosting service for DNS domains that provide name resolution by using Microsoft Azure infra. Hosting your domains in Azure means you can manager you DNS records using the same credentials, APIs, tools and billing as your other Azure services.

* What is DNS?

DNS or Domain name system is the phonebook of the internet. Meaning it performs critical function of translating user friendly names into machine readable addresses.

* **Public DNS:** Manages the records of your website, ensuring that people on the public internet can quickly and reliably find your services running in Azure by typing in the domain name.
* **Private DNS:** Manages the internal "phonebook" for your private Azure networks, allowing your VMs and services to find and communicate with each other using simple names instead of privated IP addresses.

### Why does it exist?

To provide globally distributed, highly available and fully integrated DNS hosting service for both public domains (internet facing) and your private (internal) Azure networks. It allows you to manage name resolution using the same tools and infra you use for all your cloud resources.

### What does it do?

It hosts and manages the DNS records and zones for your domains using Microsoft global infrastructure for name resolution.

* What is DNS record?
    * Fundamental piece of information stored in a DNS zone file on a DNS server. It is essentially a mapping instruction          that provides specific details like an IP address, a mail server name or text information associated with a domain           name. Every time someone accesses a website, sends an email or connects to cloud service, a DNS record is                    queried to find the correct path.

### How does it work?

It works by being an authoritative, globally distributed DNS server network that hosts your domain names and translates them into IP addresses for users, both for public internet traffic and private Azure networks.

In short Azure handles all the complexities of hosting the DNS infra, routing queries to the closest server and managing redundancy, leaving you to simply define the records.

## Benefits of Azure DNS

* Reliability and performance.
* Security.
* Ease of use
* Customizable VNets.
* Alias records.

### Summary

Azure DNS is global service that hosts your domain names like `virtualhermit.com`. Its main job is to act as a translator by taking the domain name itself and converting it into IP address so that computers can find your established server or website.

## Reliability and performance

DNS domains in Azure DNS are hosted on Azures global network of DNS name servers, which provides resiliency and high availability. Azure DNS uses anycast networking so the closest DNS server answers each DNS query, providing fast performance and high availability for your domain.

### What does it mean?

It means that Azure DNS is set up to be extremely reliable, fast and always available for finding your websites and cloud services.

## Security

Azure DNS is based on Azure resource manager with features like:

* Azure RBAC (role based access control) to control who has access to what in your organization.
* Activity logs to monitor how a user in your organization modified resources or to find an error when troubleshooting.
* Resource locking to lock a subscription, resource group or resource. Locking prevents other users in your organization from accidentally deleting or modifying critical resources.

## Ease of use

* Azure DNS can manage DNS records for your services and provide DNS for your external resources.
* Its integrated into Azure portal and uses the same credentials, support contract and biilling as your other Azure services.
* Since its running on Azure means you can manage domains and records within the portal, powershell cmdlets and Azure CLI.
* Applications that require automated DNS management can integrate with the service using REST API and SDKs.

## Customizable VNets with private domains

Azure DNS supports private DNS domains. This allows you to use your own custom domain names in your private VNet, rather than being stuck with Azure provided names.

## Alias records

Its like Azure DNS, its a special type of record that allows you to point a domain name to an Azure resource instead of a static IP address. If the IP changes then the alias record set seamlessly updates itself during DNS resolution. I.e it points to service instance and the service instance is associated with IP address.

### Why does it exist?

To ensure your domain name always points to the correct current location of Azure resource, even if the underlying IP address of tha resource changes. 

### What does it do?

Acts as a smart pinter that links doman name to dynamic cloud resource ensuring the record stays correct even if the resources underlying IP changes.

### How does it work?

It works by leveraging the Azure resource manager aka ARM integration within the Azure DNS service. Instead of statically storing an IP address, the record stores a pointer to a specific Azure resource ID.

### Summary

Alias record is like DNS that points directly to the correct location of the resource, regardless of the IP change. 
