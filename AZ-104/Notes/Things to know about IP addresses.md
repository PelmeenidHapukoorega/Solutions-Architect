# Notes about IP addresses

* IP addresses are assigned to communicate with other resources, on premises network and the internet.
* 2 types of IP addresses: `private` and `public`.

**Private IP addresses**

Enable communication within VNets and on premises networks. You can create a private IP address for your resources when you use VPN gateway or Azure ExpressRoute circuit to extend your network to Azure.

**Public IP addresses**

Allow resources to communicate with the internet. You can create public IP address to connect with Azure public-facing-services.

Diagram of Private and Public IP logic:
```
Virtual networks, VPN              Private IP            +----------+            Public IP               Internet,
  gateways, on-premises               address              |    VM    |             address              public-facing
  networks, ExpressRoute <-------------------------------->|----------|<-------------------------------->   services
                                                           | Resource |
                                                           +----------+
```

## Things to know about IP addresses

* IP addresses can be statically assigned or dynamically assigned.
* IP resources can be seperated dynamically and statically into different subnets.

* Static IP addresses dont change and are best for certain situations like:
  * DNS name resolution, where a change in the IP address requires updating host records.
  * IP address-based security models that require apps or services to have a static IP address.
  * TLS/SSL certificates linked to an IP address.
  * Firewall rules that allow or deny traffic by using IP address range.
  * Role-based virtual machines such as Domain controllers and DNS servers.
