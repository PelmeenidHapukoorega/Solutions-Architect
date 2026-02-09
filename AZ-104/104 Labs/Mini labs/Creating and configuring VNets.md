# Mini lab: Creating and configuring virtual networks

**Job Skills**

* Creating VNets
* Creating subnets
* Configuring VNet peering

Scenario:

Your organization is migrating a web-based application to Azure. Your first task is to put in place the virtual networks and subnets. You also need to securely peer the virtual networks. You identify these requirements.

* Two virtual networks are required, app-vnet and hub-vnet. The virtual networks simulate a hub and spoke network architecture.
* The app-vnet hosts the application. The app-vnet virtual network requires two subnets. The frontend subnet hosts the web servers. The backend subnet hosts the database servers.
* The hub-vnet only requires a subnet for the firewall.
* The two virtual networks must be able to communicate with each other securely and privately through virtual network peering.
* Both virtual networks should be in the same region.

Diagram:
```ascii
[ East US (RG1) ]                              [ East US (RG1) ]
+-------------------------------+              +-------------------------------+
| app-vnet (10.1.0.0/16)        |              | hub-vnet (10.0.0.0/16)        |
|                               |   VNET       |                               |
|  +-------------------------+  |  Peering     |  +-------------------------+  |
|  | frontend (10.1.0.0/24)  |  |<============>|  | AzureFirewallSubnet     |  |
|  | [ VM1 ]                 |  |              |  | (10.0.0.0/26)           |  |
|  +-------------------------+  |              |  +-------------------------+  |
|                               |              +-------------------------------+
|  +-------------------------+  |      VNET Link      +--------------------+
|  | backend (10.1.1.0/24)   |  | <---------------->  |  Private DNS Zone  |
|  | [ VM2 ]                 |  |                     | (private.contoso)  |
|  +-------------------------+  |                     +--------------------+
|                               |
|  +-------------------------+  |              +---------------------------+
|  | AzureFirewallSubnet     |  |              |  Azure Devops Pipelines   |
|  | (10.1.63.0/26)          |  |              +---------------------------+
|  | [ Firewall ]            |  |
|  +-------------------------+  |              +---------------------------+
+-------------------------------+              |  Azure DNS Resolution     |
                                               +---------------------------+
```

1. In the portal went to `Virtual networks` and created new one.
2. Next i configured the VNet and added 2 subnets `frontend` and `backend`

* IPv4 address > `10.1.0.0/16`
* Frontend subnet range > `10.1.0.0/24`
* Backend subnet range > `10.1.1.0/24`

Reviewed settings:

<img width="583" height="525" alt="image" src="https://github.com/user-attachments/assets/dca7bdd2-ae6f-41ff-ad0d-04e6b23cc313" />

Hit create.

3. Created another VNet `hub-vnet`, added same resource group and configured it:

* IPv4 address space > `10.0.0.0/16`
* Named subnet `FirewallSubnet` and gave it a range of `10.0.0.0/26`

Reviewed settings:

<img width="567" height="507" alt="image" src="https://github.com/user-attachments/assets/d0f2be2f-1046-45b4-9bd4-ce598c277b2d" />

Hit create.

4. Went to `app-vnet` network, then `settings` > `peerings` and added new peering:

* Remote peering ling > `app-vnet-to-hub`
* VNet > `hub-vnet`
* Local VNet peering link name > `hub-to-app-vnet`

Left the rest default and created the peering.

<img width="834" height="713" alt="image" src="https://github.com/user-attachments/assets/d26d6bbf-706b-4cb3-bdaf-1872d124eb08" />

Verified that peering was now connected:

<img width="1099" height="79" alt="image" src="https://github.com/user-attachments/assets/355aab97-3a56-4aa9-a503-a8a3d52061cd" />

Now VNets could communicate with each other.

## Summary

Started by creating the app‑vnet, assigning it the 10.1.0.0/16 address space, and adding the two required subnets: frontend (10.1.0.0/24) and backend (10.1.1.0/24). These represented the web tier and database tier of the application.

Created the hub‑vnet next, using the 10.0.0.0/16 address space and adding the FirewallSubnet (10.0.0.0/26). This simulated the hub portion of a hub‑and‑spoke network design.

Configured VNet peering between app‑vnet and hub‑vnet. Set up the peering from the app‑vnet side, named both peering links, and left the default settings in place. Once the connection showed as “Connected,” the two VNets could communicate privately and securely.

Verified that the architecture matched the scenario requirements: Two VNets in the same region, proper subnet layout, and secure private connectivity through peering.

## Key Takeaways

* Azure virtual networks (VNets) provide a secure and isolated network environment for your cloud resources. You can create multiple virtual networks per region per subscription.
* When designing virtual networks make sure the VNet address space (CIDR block) doesnt overlap with your organization’s other network ranges.
* A subnet is a range of IP addresses in the VNet. You can segment VNets into different size subnets, creating as many subnets as you require for organization and security within the subscription limit. Each subnet must have a unique address range.
* Certain Azure services, such as Azure Firewall, require their own subnet.
* Virtual network peering enables you to seamlessly connect two Azure virtual networks. The virtual networks appear as one for connectivity purposes.
