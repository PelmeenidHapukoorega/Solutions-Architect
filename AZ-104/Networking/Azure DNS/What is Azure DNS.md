# Notes on what is Azure DNS

* Azure DNS is a hosting service for Domain name systems (DNS) that provides name resolution using Azure infra.
* Allows to manage all domains using AZ credentials.
* Acts as SOA for the domain.
* Cant be used to register domain name, needs to be registered using 3rd party domain registrar.

## What is DNS?

* DNS is a protocol within the TCP/IP standard.
* Plays an essential role of translating the human-readable domain name lik `www.worldwideimports.com` into a know IP address.
* IP addresses enable computers and network devices to identify and route requests among themselves.
* DNS uses global directory hosted on servers around the world.

## How does DNS work?

DNS server carries out 1 or 2 primary functions:

* Maintains local cache of recently addressed or used domain names and their IP addresses.
  * This provides faster response to a local domain lookup requests.
  * If the DNS server cant find requested domain, it passes the request to another DNS server.
  * Process repeats at each DNS server until either a match is made or search times out.

* Maintains the key-value pair database of IP addresses and any host or subdomain over which the DNS server has authority.
  * This function is often associated with mail, web and other internet domain services.

## DNS server assignment

* In order for any appliance to access settings from the server, it has to reference DNS server.
* If you connect using on-prem network, the DNS settings come from your server.
* When connecting by using external location, the DNS settings come from the ISP (internet service provider).

## Domain lookup requests

Process that DNS server uses when it resolves domain-name lookup request:

* If the domain name is stored in the short-term cache, the DNS server resolves the domain request.
* If the domain isnt in the cache, it contacts 1 or more DNS servers on the web to see if they have a match.
  * When a match is found, the DNS server updates the local cache and resolves the request.
* If the domain isnt found after a reasonable number of DNS checks, the DNS server responds with a domain cannot be found error.

## IPv4 and IPv6

* **IPv4**
  * Composed of 4 sets of numbers in the range of `0-255`.
  * Commonly used standard.
  * Although with the increase of IoT the standard will eventually be unable to keep up.

* **IPv6**
  * Relatively new standard intended to replace IPv4.
  * Consists of 8 groups of hexadecimal numbers like `fe80:11a1:ac15:e9gf:e884:edb0:ddee:fea3`

Many network devices now are provisioned with both. DNS can resolve domain names for both addresses.

## DNS settings for your domain

* DNS needs to be configured for each host type being used.
  * Host types include: web, email or other services being used.

If setting up DNS server using AZ DNS the server acts as a SOA (Start of authority) for the domain.

## DNS record types

Config info of the DNS server is stored as a file within a zone of the DNS server, each file is called a record aka DNS record.

**Record types:**

* **A**
  * Host record, most common type of DNS record.
  * Maps the domain or host name to the IP address.

* **CNAME**
  * Canonical name record, used to create alias from 1 domain name to another domain name.
  * If using different domain names that all accessed the same website, you would use CNAME record.

* **MX**
  * Mail exchange record.
  * Maps mail requests for the mail server.
  * Whether hosted on-premises or in the cloud.

* **TXT**
  * Text record.
  * Used to associate text strings with a domain name.
  * AZ and MS 365 use TXT records to verify ownership of the domain.

**Additional record types:**

* Wildcards
* CAA (Certificate authority)
* NS (Name server)
* SOA (Start of authority)
* SPF (Sender policy framework)
* SRV (Server locations)

SOA and NS records are created automatically when creating DNS zone using AZ DNS.

## Record sets

* Record set allows for multiple resources to be defined in a single record.

Example of A reocord that has 1 domain with 2 IP addresses:
```
www.wideworldimports.com.     3600    IN    A    127.0.0.1
www.wideworldimports.com.     3600    IN    A    127.0.0.2
```

SOA and CNAME records cant contain record sets.

## Why use AZ DNS to host domain

* Improved security
* Ease of use
* Private DNS domains
* Alias record sets

AZ DNS now supports domain name system security extensions (DNSSEC) for public DNS zones.

## Security features

* RBAC, gives fine grained control over users access to resources.
  * You can monitor their usage and control the resources and services to which they have access.
* Activity logs let you track changes to resources and pinpoint where faults occured.
* Resource locking, gives greater level of control to restrict or remove access to resource groups, subscriptions or any AZ resource.

## Ease of use

* Uses same AZ credentials, support contract and billing as other AZ services.
* Domains and records can be manage via portal, PWSH cmdlets or the CLI
* Apps that require automated DNS management can integrate with the service using REST API and SDKs (software development kit)

## Private domains

* Provide name resolution to VMs within a VNet and between VNets without creating custom DNS solution.

Benefits:

* Supported by AZ infra, leaving out the need to invest in a DNS solution.
* All DNS records supported: A, CNAME, TXT, MX, SOA, AAAA, PTR and SRV.
* Host names for VMs in VNet are automatically maintained.
* Split-horizon DNS support allows domain name to exist in both private and public zones. It resolves to the correct one based on the originating request location.

## Alias record sets

* Can point to resources.
* Can be set up to direct traffic to public IP address, AZ traffic manager profile or AZ content delivery network endpoint.

Alias record set is supported by following record types:

* A
* AAAA
* CNAME
