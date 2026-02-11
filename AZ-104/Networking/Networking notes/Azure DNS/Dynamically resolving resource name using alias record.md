# Notes on how to dynamically resolve resource name using alias record


## Load balancers

* Load balancers distribute inbound data requests and traffic across 1 or more servers.
* They reduce load on any 1 server and imporve performance.

* A record and CNAME record dont support direct connection to resources like load balancers.

## What is apex domain?

* Apex domain is your domains highest level. In my case it was `Mmerch.com`.
* Apex domain referred to as zone apex or root apex.
* `@` symbol represents apex domain in DNS zone records.
* CNAME records arent supported at the zone apex level. Although Alias records are supported at the zone apex level.

## What are alias records

Alias records enable a zone apex domain to reference resources from the DNS zone. No need to create complex redirection policies.

Alias can be used to route traffic through traffic manager.

AZ alias records can point to the following resources:

* Traffic manager profile
* AZ content delivery network endpoints
* Public IP resource
* Front-door profile

* Alias records provide lifecycle tracking of target resources.
* Ensures that changes to any target resource are automatically applied to the DNS zone.
* Provide support for load balanced applications in the zone apex.

Supports following record types:

* A > IPv4 domain name mapping record.
* AAAA > IPv6 domain name mapping record.
* CNAME > Provides alias for the domain, links to A record.

## Uses for alias records

* **Preventing dangling DNS records**
  * Dangling DNS record occurs when DNS zone records arent up to date with changes to IP addresses.
  * Alias records prevent dangling references by tightly coupling the lifecycle of DNS record with AZ resource.

* **Updates DNS record set automatically when IP addresses change**
  * When underlying IP address of resource, service or app is changed, the alias ensures that any associated DNS records are automatically refreshed.

* **Hosts load balanced applications at the zone apex**
  * Allows for zone apex resource routing to traffic manager.

* **Points zone apex to AZ content delivery network endpoints**
  * You can directly reference AZ content delivery network instance.

It allows you to link the zone apex to a load balancer. It creates a link to AZ resource rather than a direct IP based conneciton.

Meaning that if IP address changes for the load balancer, the zone apex record continues to work.
