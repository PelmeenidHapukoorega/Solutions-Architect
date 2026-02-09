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

