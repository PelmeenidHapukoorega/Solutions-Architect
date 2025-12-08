# Describe Azure ExpressRoute

## Table of contents
* [ExpressRoute circuit](#expressroute-circuit)
* [Features and benefits of ExpressRoute](#features-and-benefits-of-expressroute)

## My interpretations

Azure ExpressRoute lets you extend your on prem networks into cloud over a private connection, with the help of a connectivity provider. This is called ExpressRoute circuit. With ExpressRoute you can establish connection to cloud services such as Azure and Microsoft 365. This feature allows you to connect to offices, data centers or other facilities to the Microsoft cloud. Each location would have its own ExpressRoute circuit.

### Why does it exist?

To provide organizations with a private, dedicated high speed and reliable connection between on prem network and the Microsoft cloud. It serves as a better alternative to connecting over the public internet, solving issues related to performance, security and reliability, but its also expensive.

### What does it do?

Provides private, secure and highly reliable connection between users on prem network and Microsofts global cloud network.

### How does it work?

By using dedicated, physical circuit provisioned by a connectivity provider to establish a private connection between your on prem network and Microsoft global network edge. Connectivity provider being either Telco, network service provider or cloud exchange provider which you pick yourself.

### Summary

ExpressRoute is an alternative for VPN, its faster, more reliable and less management but also quite expensive in contrast to setting up VPNs.

## ExpressRoute circuit

ExpressRoute circuit is the logical resource you create and manage in the Azure portal. It represents the actual, physical connection delivered by the connectivity provider.

### Why does it exist?

It exists because its fundamental logical resources required to represent and manage the physical dedicated connection that links your on prem network to Microsoft cloud.

### What does it do?

Its a single logical control panel within Azure that manages and defines the physical, high speed private connection between your on prem network and the cloud.

### How does it work?

* Defining the bandwidh and locations where you speficiy bandwidh capacity (500mbs, 1gb, 10gb) and the geographical peering location where the physical connection terminates into Microsoft network.
* Authorizes the connection by generating a unique service key aka S-key. This key acts as the authorization token you provide to your connectivity provider telling them to connect your physical equiptment to that specific circuit resources in Azure.
* Configures traffic access aka Peering: Then it enables and configures different routing domains aka peerings that control what service you can access, each using independent BGP sessions for high availability.
* It is provisioned with built in redundancy (2 physical links to 2 different microsoft edge routers) and uses the BGP protocol to dynamically exchange network routes with your on premises router.

### Summary

The circuit does the job of defining, authorizing and configuring high performance private link to the cloud. I.e its the bluepring for connectivity providers so they know what needs to be done and how.

### Features and benefits of ExpressRoute

* Connectivity to Microsoft cloud services across all regions and geopolitical regions.
* Global connectivity to Microsoft services across all regions with the ExpressRoute Global reach.
* Dynamic routing between network and Microsft via Border gateway protocol aka BGP.
* Built in redundancy (duplication) in every peering location for higher availability.

### Connectivity to Microsoft cloud services

* Microsoft office 365
* Microsoft dynamics 365
* Azure compute services (VMs, resource groups etc)
* Azure cloud services such as Azure cosmos DB and Azure storage.

### Global connectivity

You can enable ExpressRoute Global reach so can exchange data across your on prem sites by connecting your Circuits.
      * Example: Lets say you have office in Europe and a data center in America, both with ExpressRoute circuits                   connecting them to Microsoft network. You could use it to connect those 2 facilities allowing them to communicate           without transferring data over the public internet.

### Built in redundancy

Each connectivity provider uses redundant devices to ensure that connections established with Microsoft are highly available. You can configure multiple circuits to complement this feature.

What does it mean?

It means that high reliability of your ExpressRoute is achieved through layers of redundancy, starting with physical devices used by the connectivity provider and extending into your own Azure configuration.

## ExpressRoute connectivity models

ExpressRoute supports four models which you can use to connect your on prem network to cloud:

* Cloudexchange colocation.
* P2P ethernet connection.
* A2A (any to any) connection.
* Directly from ExpressRoute sites.

### Colocation at a cloud exchange

Colocation refers to your data center, office or another facility being physically colocated at a cloud exchange, such as an ISP (Internet service protocol). If your facility is colocated at a cloud exchange, you can request virtual cross connect to the cloud.

### What does it mean?

You move critical hardware into the same data center building where Azures connection points are located and then use a simple cable to plug directly into the cloud.

### Point to point ethernet connection

Point to point ethernet connection refers to using a point-to-point connection to connect your facility to the Microsoft cloud.

### What does it mean?

It means you get a private, dedicated physical cable run from your business to the nearest cloud access point and then you use that cable to carry your data directly and securely to the Azure network.

### Any to any networks

With any to any you can integrate you WAN (wide are network) with Azure by providing connections to your offices and data centers. Azure integrates with you WAN connection to provide a connection like you would have between your data center and any branch offices.

### What does it mean?

This method simplifies global networking by using Azures backbone to a create single unified and easily manageable network where all sites, physical and cloud can talk to each other without complex manual configurations.


### Directly from ExpressRoute sites

You can connect directly into Microsofts global network at a peering location strategically distributed across the world. ExpressRoute provides dual 100 Gbps or 10Gbps connectivity, which supports active/active connectivity at scale.

### What does it mean?

It means ExpressRoute offers a way to directly and reliably plug your network into Microsoft wordlwide backbone, providing massive bandwidth and built in redundancy for critical enterprise needs.

### Security considerations

* With ExpressRoute your data doesnt travel over the public internet, which minimizes risk associated with internet comms.
* Its a private connection from your on prem infra to Azure infra.
* Even if you have ExpressRoute connection, DNS queries, Azure content delivery Network, requests are still sent over the public internet.
