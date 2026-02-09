## M01: Designing and implementing virtual network in Azure

### Goal:

Implement 3 virtual networks and subnets to support resources in those virtual networks.

**Job Skills**

* Resource group creation
* Creating CoreServicesVnet virtual network and subnets
* Creating ManufacturingVnet virtual network and subnets
* Creating ResearchVnet virtual network and subnets
* Verify the creation of VNets and Subnets

Diagram:
```ascii
[ On-premises site ]
                                (10.10.0.0/16)
                                      |
                                      | IPsec IKE S2S
                                      | VPN tunnel
                                      |
       SOUTHEAST ASIA              EAST US               WEST EUROPE
    +------------------+      +------------------+      +------------------+
    |   ResearchVnet   |      | CoreServicesVnet |      |ManufacturingVnet |
    |  (10.40.0.0/16)  |      |  (10.20.0.0/16)  |      |  (10.30.0.0/16)  |
    |------------------|      |------------------|      |------------------|
    | ResearchSystem   |      | GatewaySubnet    |      | Manufacturing    |
    | Subnet           |      | 10.20.0.0/27     |      | SystemSubnet     |
    | 10.40.0.0/24     |      |                  |      | 10.30.10.0/24    |
    +---------+--------+      | DatabaseSubnet   |      |                  |
              |               | 10.20.20.0/24    |      | SensorSubnet1    |
           Peering            |                  |      | 10.30.20.0/24    |
              |               | SharedServices   |      |                  |
              |               | Subnet           |      | SensorSubnet2    |
              |<------------->| 10.20.10.0/24    |<---->| 10.30.21.0/24    |
                              |                  |      |                  |
                              | PublicWebService |      | SensorSubnet3    |
                              | Subnet           |      | 10.30.22.0/24    |
                              | 10.20.30.0/24    |      +------------------+
                              +------------------+
```

1. Created resource grouo `Contoso` in Eastu US region
2. Went to `Virtual networks` and created a new one with `10.20.0.0/16` as IPv4 address space and named it `CoreServicesVnet`
3. Added following subnets under `IP addresses` tab:

**GatewaySubnet**

* Set the value to `VNet gateway`
* Named was added automatically `GatewaySubnet`
* Set the range to `10.20.0.0/27`

**SharedServicesSubnet**

* Set the range to `10.20.10.0/24`

**DatabaseSubnet**

* Set the range to `10.20.20.0/24`

**PublicWebServiceSubnet**

* Set the range to `10.20.30.0/24`

Validated and reviewed settings:

<img width="684" height="732" alt="image" src="https://github.com/user-attachments/assets/7f018232-d39b-4f42-a06c-700d8a2737d5" />

Hit create.

4. Next i created `ManufacturingVnet` in the West Europe region with `10.30.0.0/16` IPv4 address space.
5. Added following subnets:

**ManufacturingSystemSubnet**

* Set the range to `10.30.10.0/24`

**SensorSubnet1** 

* Set the range to `10.30.20.0/24`

**SensorSubnet2**

* Set the range to `10.30.21.0/24`

**SensorSubnet3**

* Set the range to `10.30.22.0/24`

Validated and reviewed settings:

<img width="663" height="709" alt="image" src="https://github.com/user-attachments/assets/38e31b8d-13b6-4655-a915-7a51274b750a" />

Hit create.

6. Next i created `ResearchVnet` in the Southeast Asia region with `10.40.0.0/16` IPv4 address space.

Added `ResearchSystemSubnet` with range of `10.40.0.0/24`

Validated and reviewed settings:

<img width="565" height="629" alt="image" src="https://github.com/user-attachments/assets/d7a36c42-d82f-4259-82a5-3fc6c3b1a7e6" />

Hit create.

## Verified creation of VNets and subnets

**CoreServicesVnet and its subnets:**

<img width="903" height="417" alt="image" src="https://github.com/user-attachments/assets/fde7b0d7-9041-4719-9a8d-e48a85dc8b66" />

**ManufacturingVnet and its subnets:**

<img width="899" height="405" alt="image" src="https://github.com/user-attachments/assets/e9d2e5f5-136e-4f8e-9a96-eb13a33a21f6" />

**ResearchVnet and its subnets:**

<img width="894" height="295" alt="image" src="https://github.com/user-attachments/assets/795c0bb0-6e0d-49cf-a932-c72eba223df7" />

### Summary

Started by creating a new resource group called Contoso in East US to hold all three virtual networks. This gave me a clean, organised place to deploy the environment.

Created the `CoreServicesVnet` with the 10.20.0.0/16 address space. Added 4 subnets: GatewaySubnet, SharedServicesSubnet, DatabaseSubnet, and PublicWebServiceSubnet. This VNet represented the central services layer of the architecture.

Built the `ManufacturingVnet` in West Europe using the 10.30.0.0/16 address space. Added the ManufacturingSystemSubnet and 3 sensor subnets, matching the design for distributed manufacturing workloads.

Created the `ResearchVnet` in Southeast Asia with the 10.40.0.0/16 address space and added the ResearchSystemSubnet. This completed the global layout of the three VNets.

Verified each VNet and its subnets to ensure the address spaces and subnet ranges matched the design diagram. All VNets were successfully deployed with the correct structure.

### What i learned

* Learned how to design and deploy multiple VNets across different regions.
* Learned how to plan and assign address spaces and subnet ranges without overlap.
* Learned how subnet naming and structure reflect real workload seperation.
* How to validated VNet deployments and confirm that the architecture matches the intended design.

### Key Takeaways

* Proper IP planning is essential when building multi VNet architectures.
* Subnets help isolate workloads such as databases, shared services and sensors.
* VNets can span multiple regions to support global architectures.
* Verifying address spaces and subnet ranges prevents routing conflicts later.

From MS Learn for reminders:

* Azure Virtual Network is a service that provides the fundamental building block for your private network in Azure. An instance of the service (a virtual network) enables many types of Azure resources to securely communicate with each other, the internet, and on-premises networks. Ensure nonoverlapping address spaces. Make sure your virtual network address space (CIDR block) doesn’t overlap with your organization’s other network ranges.
* All Azure resources in a virtual network are deployed into subnets within the virtual network. Subnets enable you to segment the virtual network into one or more subnetworks and allocate a portion of the virtual network’s address space to each subnet. Your subnets shouldn’t cover the entire address space of the virtual network. Plan ahead and reserve some address space for the future.
