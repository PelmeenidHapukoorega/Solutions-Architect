# Lab 5: Implementing intersite connectivity

**Job skills**

* Creating virtual machine in a VNet
* Creating virtual machine in a different VNet
* Using network watcher to test connection between virtual machines
* Configuring virtual network peerings between virtual machines
* Using Azure powershell to test the connection between virtual machines
* Creating custom route

## Part 1: Creating core services VM

1. Went to `Virtual machines` landing page and selected `create`.
2. On the basics tab added name for the VM and resource group where it will be held, security type was set to standard. Picked windows server 2025 x64 gen 2 as image, for size i went with Standard D2s v3 which seemed like a decent pick when it comes to cost and size. Added a username `localadmin` as well as the password. For public inbound ports left it at none because if enabled it allows outside traffic to reach service inside the network.

On the `disks` tab left everything at default.

On the `Networking` i created a new VNet with following values:

* Name > CoreServicesVnet
* Address range > 10.0.0.0/16
* Subnet name > Core
* Subnet address range > 10.0.0.0/24

Went to `Monitoring` and disabled `Boot diagnostics`. This meant i didnt need to wait for resources to be created and could continue on moving to the next task.

Hit `Create`

<img width="867" height="396" alt="image" src="https://github.com/user-attachments/assets/339872e3-f954-4c75-8e9a-f893b7193206" />

Basically what i did here in a nutshell was a Virtual machine and VNet at the same time.

## Part 2: Creating virtual machine in a different VNet

1. Went back to `Virtual machines` named it and left the values same as for the last VM. Default for `Disks` and under `Networking` added following:

<img width="1015" height="783" alt="image" src="https://github.com/user-attachments/assets/35288807-1d17-42ba-bb30-3d781f848516" />

Selected `review + create` then `create` when all seemed well.

<img width="467" height="414" alt="image" src="https://github.com/user-attachments/assets/a7f9b952-2d0c-4f2a-819f-e6db99443ffb" />

## Part 3: Using network watcher to test connection between VMs

1. Searched up `network watcher` in the portal then from blades menu `network diagnostic` > `connection troubleshoot`
2. Added values from which VM to which VM i wanted to test the connection, added destination port `3389`. Why 3389 port? Port 3389 is considered communication channel for RDP aka remote desktop protocol. It provides a clea measurable way to verify that VNet peering is actually working.

Left the rest defauly and ran diagnostics:

<img width="719" height="456" alt="image" src="https://github.com/user-attachments/assets/8a8a39f6-3180-4e23-8358-da2d244bbe0c" />

Naturally it failed because VMs are in different VNets and in order for them to be able communicate with each other, peering was needed between them.

## Part 4: Configuring VNet peerings between VNets

1. Went to `CoreServicesVnet` VNet i created earlier, then `Settings` > `Peerings` > `+ add`

Under peering link name provided value with to/from logic `ManufacturingVnet-to-CoreServicesVnet`, selected ManufacturingVnet i created. Then enabled allow access for coreservicesvnet to be able to access manufacturingvnet as well as allow to recieve traffic forwarded from manufacturing vnet.

Then did the vice versa for coreservicesvnet with the exception of name `CoreServicesVnet-to-ManufacturingVnet`

Selected `add`

2. Went to `CoreServicesVnet` and checked under `Peerings` to verify that peering was now listed:

<img width="1606" height="106" alt="image" src="https://github.com/user-attachments/assets/d2fbc3c5-9c6a-499a-84f6-5afa74ace454" />

Did the same for `ManufacturingVnet`:

<img width="1594" height="94" alt="image" src="https://github.com/user-attachments/assets/80d54790-5335-4e18-924e-7fd967a4dfa7" />

## Part 5: Using Azure Powershell to test connection between VMs

1. Went to `Virtualmachines` resource group and opened up `CoreServicesVM` > `Networking` and copied Private IP address of the machine itself since i needed it for a command.

2. Went to `Manufacturing` VM and selected `Operations` blade > `Run command`. Selected `RunPowerShellScript` to test the connection:
```PWSL
Test-NetConnection 10.0.0.4 -port 3389
```

Result:

<img width="569" height="537" alt="image" src="https://github.com/user-attachments/assets/d6896a11-bef0-4ce2-b06b-a40f0870f839" />

Peering was configured successfully and connection test went through. On a sidenote i didnt know you could just pick a VM from your selection and run commands like that in the portal, that was pretty nice addition to find out, means i dont have to select specific VM with commands from the CLI but i could just cut straight to VM i need and run a command i need. Neat addition.

## Part 6: Creating custom route

1. Went to `CoreServicesVnet` > Subnets and added new subnet:

Added name `Perimeters` and set starting address `10.0.1.0/24`

2. Went to `Route tables` > `+ Create`:

<img width="469" height="494" alt="image" src="https://github.com/user-attachments/assets/4c6e3185-bb92-4143-9eaf-e191267c1310" />

Added values and selected `Create`

<img width="660" height="433" alt="image" src="https://github.com/user-attachments/assets/68e434d6-527d-4d63-af31-6dfb6ea21f6e" />

3. Went to the route table resource, expanded `settings` > `Routes` > `+ add`. Then proceeded to create route from a future NVA(Network virtual appliance) to CoreServices VNet:

<img width="563" height="565" alt="image" src="https://github.com/user-attachments/assets/dba545aa-33f3-4c97-9d01-9d0e2a6ed638" />

Destination IP is CoreServiceVnets address space aka scope as in where the route is being created.

Selected `+ add`. Then i needed to associate the route with the subnet.

4. Selected `Subnets` of `rt-CoreServices` > `+ Associate` and completed the configuration:

<img width="663" height="153" alt="image" src="https://github.com/user-attachments/assets/b964f759-fc47-49da-a3d7-61b1cf044943" />


I now had created user defined route to direct traffic from DMZ to the new NVA

### Summary

Learned how to create a couple VMs in different virtual networks `CoreServicesVnet` and `ManufacturingVnet` and realized they cant talk to each other by default even if they are in the same region.

Additionally noticed that when i ran a connection test through Network Watcher using port 3389, it failed straight away because there was no bridge between the networks. I practiced fixing this by configuring VNet peering with that to/from logic like ManufacturingVnet-to-CoreServicesVnet which finally opened up the communication channels. It was cool to see that once peering was enabled, i could run a `Test-NetConnection` command directly from the portals `Run command` blade and see it actually succeed.

Also practiced setting up a custom route by creating a new Perimeters subnet and a route table called `rt-CoreServices`. This taught me how to manually direct traffic to a Network Virtual Appliance (NVA) by adding a user defined route and associating it with the right subnet. Lab showed me that VNet peering and custom routing are the way to go when you need to connect different parts of your cloud infrastructure securely.

### Key Takeaways

* By default resources in different VNets cannot communicate.
* VNet peering enables you to seamlessly connect 2 or more VNets in Azure.
* Peered VNets appear as 1 for connectivity purposes.
* Traffic between VMs in peered VNets uses the microsoft backbone infrastructure.
* System defined routes are automatically created for each subnet in a VNet. User defined routes override or add to the default system routes.
* Azure network watcher provides a suite of tools to monitor, diagnose and view metrics/logs for Azure IaaS resources.
