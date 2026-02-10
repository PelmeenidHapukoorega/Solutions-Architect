# M01 Unit 8: Connecting 2 VNets using Global VNet peering

### Goal:

Configuring connectivity between `CoreServicesVnet` and `ManufacturingVnet` by adding peerings to allow traffic flow between them.

**Job Skills**

* Creating VM to test configuration
* Connecting to the Test VMs using RDP
* Testing connection between the VMs
* Creating VNet peerings between `CoreServicesVnets` and `ManufacturingVnet`
* Testing connection between VMs

Diagram:
```ascii
______________________________________________________________________________________________
| [ DragonEggs ]                                                                             |
|                                                                                            |
|          [ East US ]                            [ West Europe ]                            |
|  +-------------------------+          +------------------------------------------+         |
|  | CoreServicesVnet        |          | ManufacturingVnet (10.30.0.0/16)         |         |
|  | (10.20.0.0/16)          |          |  +------------------------------------+  |         |
|  |                         |   VNet   |  | ManufacturingSystemsSubnet         |  |         |
|  |      [ TestVM1 ]        |<-------->|  | (10.30.10.0/24)                    |  |         |
|  |      (10.20.20.4)       | Peering  |  | [NSG: ManufacturingVM-nsg]         |  |         |
|  |           ^             |          |  | [ ManufacturingVM ] <--------------|--|---[ User ]
|  +-----------|-------------+          |  | (10.30.10.4)                       |  |   (Laptop)
|              |                        |  +------------------------------------+  |     RDP
|              └------- Test RDP Session ------------------------------------------┘   Session
|____________________________________________________________________________________________|
```

1. Downloaded `ManufacturingVMazuredeploy.json` and `ManufacturingVMazuredeploy.parameters.json` then uploaded both to terminal.
2. Set the variable for resource group with `$RGName = "DragonEggs"`
3. Ran the template deployment for VM:
```PWSH
New-AzResourceGroupDeployment `
-ResourceGroupName $RGName `
-TemplateFile ManufacturingVMazuredeploy.json `
-TemplateParameterFile ManufacturingVMazuredeploy.parameters.json `
```

<img width="813" height="406" alt="image" src="https://github.com/user-attachments/assets/8c7c27cc-eff9-461e-9976-89a6a0c59094" />

Verified VM creation in the portal by going to `Virtual machines`:

<img width="603" height="38" alt="image" src="https://github.com/user-attachments/assets/9c86a2e0-7c6e-4b86-8f41-e7f0ed9dfa91" />

3. Selected `ManufacturingVM` > `Connect` > `RDP` and downloaded RDP file
4. Logged into the VM.
5. Then selected `TestVM1` and did the same thing.
6. Once both VMs were logged into i went to `TestVM1` and opened up shell to run `ipconfig` and noted the IPv4 address `10.20.20.5`.

## Testing connection between VMs

1. In the `ManufacturingVM` opened up shell and verified that there was no connection to `TestVM1` on `CoreServicesVnet` by using the IP address i got from `TestVM1`:
```PWSH
Test-NetConnection 10.20.20.4 -port 3389
```

<img width="464" height="287" alt="image" src="https://github.com/user-attachments/assets/f443c26c-d1f3-4d00-8c16-796ddcabcee7" />

Failed as i assumed since peering wasnt enabled yet.

## Creating VNet peerings between CoreServicesVnet and ManufacturingVnet

1. Went to `Virtual networks` in the portal, selected `CoreServicesVnet`, then `settings` > `Peerings`.
2. Added new peering, named the peering link to `ManufacturingVnet-to-CoreServicesVnet` and selected `ManufacturingVnet`
3. In remote VNet peering settings made sure that:

* Allow `ManufacturingVnet` to access `CoreServicesVnet` was enabled
* `ManufacturingVnet` to receive forwarded traffic from `CoreServicesVnet` was enabled
* Did the same for `CoreServicesVnet`

4. Verified that `CoreServicesVnet-to-ManufacturingVnet` peering said `connected`:

<img width="1110" height="89" alt="image" src="https://github.com/user-attachments/assets/a98f191a-6530-4e1f-9c52-8360cbe8cb91" />

Then verified the same for `ManufacturingVnet`:

<img width="1029" height="79" alt="image" src="https://github.com/user-attachments/assets/dd63d899-e026-4e5a-ab90-d279a27a53ae" />

5. Tested the connection again in `ManufacturingVM`:
```PWSH
 Test-NetConnection 10.20.20.4 -port 3389
```

<img width="550" height="218" alt="image" src="https://github.com/user-attachments/assets/5dcf0e20-6383-4d4f-b006-00fa84688776" />

Now both VMs could communicate with each other through VNet peering.

Performed cleanup by running resource group deletion in terminal:
```PWSH
Remove-AzResourceGroup `
-Name 'DragonEggs' -Force -AsJob
```

### Summary

For this unit i had to get two different VNets (CoreServices and Manufacturing) to talk to each other even though they are in completely different regions: East US and West Europe.

Started by spinning up the Manufacturing VM using JSON templates and powershell like before. Once that was up i tried to run a `Test-NetConnection` on port 3389 from one VM to the other but it failed immediately because they werent peered yet.

Then i went into the portal and set up the VNet peering between them. Had to make sure both sides were set to allow access and receive forwarded traffic. Once both VNets showed "Connected," i ran the test command again and it worked! They could finally see each other across the global Azure backbone. After that i just nuked the whole `DragonEggs` resource group to save money.

### What i learned

* Even if VNets are in the same subscription or resource group, they cant talk to each other by default unless you link them.
* Global VNet peering is what you use when your networks are in different regions (like East US to West Europe).
* When setting up peering, you have to configure it on both ends for the traffic to actually flow both ways.
* `Test-NetConnection` with the `-port 3389` flag is a much better test than just a regular ping because it checks if the specific RDP path is open.
* Using the `-AsJob` flag when deleting a resource group is a life saver because it runs the cleanup in the background so i dont have to sit there staring at the terminal.

### Key takeaways from MS learn

* Virtual network peering enables you to seamlessly connect two Azure virtual networks. The virtual networks appear as one for connectivity purposes.
* Azure supports connecting virtual networks within the same Azure region and across Azure regions (global).
* The traffic between virtual machines in peered virtual networks is routed directly through the Microsoft backbone infrastructure, not through a gateway or over the public Internet.
* You can resize the address space of Azure virtual networks that are peered without incurring any downtime on the currently peered address space.
