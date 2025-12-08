# Describe Azure DNS

## Table of contents
* [ExpressRoute circuit](#expressroute-circuit)
* [Features and benefits of ExpressRoute](#features-and-benefits-of-expressroute)

## My interpretations

Azure DNS is a hosting service for DNS domains that provide name resolution by using Microsoft Azure infra. Hosting your domains in Azure means you can manager you DNS records using the same credentials, APIs, tools and billing as your other Azure services.

* What is DNS?

DNS or Domain name system is the phonebook of the internet. Meaning it performs critical function of translating user friendly names into machine readable addresses.

### Why does it exist?

To provide globally distributed, highly available and fully integrated DNS hosting service for both public domains (internet facing) and your private (internal) Azure networks. It allows you to manage name resolution using the same tools and infra you use for all your cloud resources.

### What does it do?

It hosts and manages the DNS records and zones for your domains using Microsoft global infrastructure for name resolution.

* What is DNS record?
    * Fundamental piece of information stored in a DNS zone file on a DNS server. It is essentially a mapping instruction    that        provides specific details like an IP address, a mail server name or text information associated with a domain name. Every       time someone accesses a website, sends an email or connects to cloud service, a DNS record is queried to find the correct       path.

### How does it work?

It works by being an authoritative, globally distributed DNS server network that hosts your domain names and translates them into IP addresses for users, both for public internet traffic and private Azure networks.

