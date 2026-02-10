# Notes about configuring public IP services

* Public networks like the internet communicate by Public IP addresses.
* Private networks like VNets use Private IP addresses which arent routable on public networks.

To support a network that exists both in Azure and On-Premises means that IP addressing needs to be configured for both types.

* Public IP addresses enable Internet resources to communicate with Azure resources.
* Enable resources to communicate outbound with internet and public-facing Azure services.
* Public IP address is dedicated to a specific resource.
* Resource without public IP assigned can communicate outbound through network address translation services where Azure dynamically assigns an available IP address that isnt dedicated to the resource.

**Example:**

Public resources like web servers must be accessible from the internet. You want to ensure that you plan IP addresses that support the requirements.

## Using dynamic and static public IP addresses

In ARM a public IP address is a resource that has its own properties.

Some of the resources can be associated a Public IP address resource with:

* Virtual machine network interfaces
* Virtual machine scale sets
* Public load balancers
* Virtual network gateways (VPN/ER)
* NAT gateways
* Application gateways
* Azure firewall
* Bastion host
* Route server

Public IP addresses are created with `IPv4` or `IPv6` address. Can be either `static` or `dynamic`.

* **Dynamic Public IP address:**
  * When assigned can change over the lifespan of the resource.
  * Is allocated when you create or start a VM.
  * IP address is released when VM is stopped or deleted.
  * Each region has a unique pool of addresses where the Public IP addresses are assigned from.
  * Default allocation method is dynamic.

* **Static Public IP address:**
  * Assigned address that is fixed over the lifespan of the resource.
  * Ensures that the IP address for the resource remains the same.
  * When setting allocation method to static an IP address is assigned immediately.
  * IP address is released only when deleting the resource or changing IP allocation method to dynamic.

## Choosing appropriate SKU for public IP address

<img width="738" height="711" alt="image" src="https://github.com/user-attachments/assets/603c7bf1-9b4e-4270-9daa-a730332e6f9a" />
