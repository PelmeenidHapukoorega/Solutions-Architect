# Notes about Design name resolution for virtual network

Azure provides both Public and Private DNS services.

## Public DNS services

* Public DNS is a hosting service for DNS domains that provide name resolution using MS infrastructure.
* DNS domains are hosted on globale network of DNS name servers.
* Each DNS query is directed to the closest available DNS server.
* Azure DNS service manages and resolves domain names in a VNet without needing to add custom DNS solution.

## Configuration considerations

* Name of the DNS zone has to be unique within the resource group and zone must not exist already.
* Same zone name can be reused in a different RG or a different subscription.
* Where multiple zones share the same name, each instance is assigned different name server addresses.
* Root/parent zone is registered at the registrar and pointed to Azure NS name servers.

## Delegating DNS domains

* AZ DNS allows to host DNS zone and manage DNS records for a domain in AZ.
* Domain must be delegated to AZ DNS from parent domain for DNS queries to reach AZ DNS.
* AZ DNS is not the domain registrar.
* In order to delegate, you need to know name server names for the zone.
* Everytime DNS zone is created, AZ DNS allocates name servers from a pool.
* When Name Servers are assigned, AZ DNS automatically creates authoritative NS (Name Server) records in the zone.
* When DNS zone is created and you have name servers, you need to updated the parent domain.
* Each registrar has their own DNS management tools to change NS records for a domain.
* In the registrars DNS management page, edit NS records and replace them with the ones AZ DNS created.

**Note:**

When delegating a domain to AZ DNS, you have to use name server names provided by AZ DNS. Always use all 4 name server names regardless of the name of the domain.
