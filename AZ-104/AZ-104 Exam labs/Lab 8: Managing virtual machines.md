# Lab 8: Managing virtual machines

## Goal:

Create and compare VMs to VM scale sets. Learning how to configure and resize single VM, how to create scale sets and configure autoscaling. 
```ascii
+---------------------------------------+
|                TASK 1                 |
|                                       |
|  [ az104-vm1 ]         [ az104-vm2 ]  |
|     Zone 1                Zone 2      |
+-------------------+-------------------+
                    |
                    v
+---------------------------------------+
|                TASK 2                 |
|                                       |
|           resize and update           |
+---------------------------------------+
```

**Job Skills**

* Deploying zone resilient VMs using portal.
* Managing compute and storage scaling for VMs.
* Creating and configuring VM scale sets.
* Scaling VM scale sets.
* Creating VM using PWSH.
* Creating VM using CLI.

## Part 1: Deploying zone resilient VM using portal

1. Went to `Virtual machines` created new one.
2. On the `Basics` tab:

* Added resource group `az104-rg8` where the VMs would be created.
* For region i selected norwayeast because eastus didnt support `Standard D2s v3` size required for the lab.
* Named VM `az104-vm` then under `availability zone` i checked `zone 2` in addition to `zone 1` which was checked by default. Noticed that when you name your VM and check other zones it will automatically name the subsequent VM the same way. So i my context i named vm `az104-vm` but once i checked zone 2 names changed to `az104-vm1, az104-vm2`.
* For image selected `windows server 2025 datacentre x64 gen 2`.
* Added `localadmin` as username and provided password.
* Unselected public inbound ports.

3. On the `Disk` tab:

* Made sure `premium SSD` was selected.
* Delete with VM was checked.
* Ultra disk compatibility was unchecked.

4. In the `Networking` took defaults but:

* Checked `delete public IP and NIC when VM is deleted`.
* Selected none for `load balancing options`.

5. In the `Monitoring` disabled `boot diagnostics`.
6. Took defaults in `advanced` went for final review:

<img width="565" height="533" alt="image" src="https://github.com/user-attachments/assets/4c30944e-9a0f-44ec-854a-2e8e39eefe8a" />

<img width="411" height="173" alt="image" src="https://github.com/user-attachments/assets/7d777772-947e-49e2-91d7-6d401f47488c" />

<img width="309" height="158" alt="image" src="https://github.com/user-attachments/assets/bd5c78a3-391d-4941-b24b-b108b564646b" />

Created VMs.

## Part 2: Managing compute and storage scaling for the VMs

### Goal:

Had to scale VMs by adjusting their size to different SKU. 

1. Went to `az104-vm1` > `availabilit +scale` > `size` > selected `D2ds_v4` from the `D-series v4` and selected `resize` at the bottom:

<img width="907" height="225" alt="image" src="https://github.com/user-attachments/assets/62d387b1-1295-4c9a-a3b4-02df6f6db0ff" />

2. Went to `settings` > `disks` > `+ create and attach a new disk`.

Named it `vm1-disk1`, for storage type took `standard HDD` with 32 Gib as size:

<img width="638" height="144" alt="image" src="https://github.com/user-attachments/assets/99ea4621-96db-4b2b-a7e9-33d39277117c" />

Then detached it by going to the right of the disk:

<img width="169" height="109" alt="image" src="https://github.com/user-attachments/assets/ca74fe30-1a56-4716-83bf-00022d804632" />

Selected `apply`.

Went to `Disks` and selected `vm1-disk1` object. Then `settings` > `size + performance` and set the storage type to `standard ssd` and hit save.

<img width="937" height="890" alt="image" src="https://github.com/user-attachments/assets/a8eed3ee-e913-4687-9fad-6775a81da8b1" />

<img width="356" height="61" alt="image" src="https://github.com/user-attachments/assets/bc44bbe5-7d1c-4e67-bf89-d6ed5b3fb39a" />

Navigated back to `az104-vm1` > `disks` and attached the existing disk `vm1-disk1`.

Verified the disk was now `standard SSD`:

<img width="480" height="123" alt="image" src="https://github.com/user-attachments/assets/adcca500-76d5-4f4e-a613-71ebd0a34f1c" />

Hit apply.

## Part 3: Creating and configuring VM scale sets

### Goal:

Deploy VM scale set across availability zones to reduce admin overhead of automation by enabling me configure metrics or conditions that would allow the scale set to horizontally scale, scale in or scale out.

```ascii
+---------------------------------------+
|                TASK 3                 |
|                                       |
|               [ vmss1 ]               |
|             Zone 1, 2, 3              |
+-------------------+-------------------+
                    |
                    v
+---------------------------------------+
|                TASK 4                 |
|                                       |
|        Custom auto scale rules        |
+---------------------------------------+
```

1. Went to `virtual machine scale sets` and created new one.
2. In the `basics` tab:

* Selected my resource group `az105-rg8`
* Named the sscale set to `vmss1`
* Region norwayeast (sames as the VMs)
* Orchestration mode > Uniform
* Security type > Standard (basic security since im not doing anything fancy)
* Scaling options left default > 	Manually update the capacity: Maintain a fixed amount of instances.
* For image i selected the same one as for the VMs.
* Size > Standard D2s_v3
* Username `localadmin` and added password.

3. Took defaults in `spot` tab and same in `disks`
4. In `networking` > `virtual network` > edited the virtual network:

* named it `vmss-vnet`
* Address range `10.82.0.0/20` deleted the default one prior.
* Added subnet `subnet0` and gave it a range `10.82.0.0/24` then hit save.

4. Edited `edit network interface` aka NIC:

<img width="801" height="128" alt="image" src="https://github.com/user-attachments/assets/35c1f58e-c86a-440e-bc93-a8d5744855d1" />

5. For NIC security group selected `advanced` then > `create new` under `configure network security group from the drop down list.

Added new inbound rule with following values:

* Source > any
* source port ranges > *
* Destination > any
* Service > HTTP
* Action > allow
* Priority > 1010
* Name > allow-http

Selected add, then on `create network security group` > OK:

<img width="547" height="886" alt="image" src="https://github.com/user-attachments/assets/92c5208f-c1f3-4dda-a69c-bff42c243531" />

Then in `edit network interface` enabled public ip address and hit OK:

<img width="429" height="164" alt="image" src="https://github.com/user-attachments/assets/2ef48265-9b17-4088-9d34-482cd8022364" />

6. In the networking tab made sure `azure load balancer` was selected under `select load balancer` > created new one and named it `vmss-lb`. Left the rest at default.

By now i had configured VM scale set with disks and networking. In the network configuration i created NSG and allowed traffic through HTTP, then i created load balancer with public IP address.

7. Made sure boot diagnostics was disabled in `management` tab.
8. In the `health` tab took defaults then went for final review before deploying.

Final review:

<img width="755" height="505" alt="image" src="https://github.com/user-attachments/assets/15ee7daa-f8aa-456f-9bcc-7ce6bc9569b7" />

<img width="503" height="543" alt="image" src="https://github.com/user-attachments/assets/2de8f4f0-f705-4fe5-9b97-cc3fc12fefe9" />

Went ahead and created scale set.

<img width="590" height="306" alt="image" src="https://github.com/user-attachments/assets/4bf267a5-469f-4a90-a37e-43ddc14978f0" />

## Part 4: Scaline VM scale sets

1. Went to `vmss1` scale set > `availability + scale` > `scaling`
2. Selected `custom autoscale` > changed `scale mode` to `scale based on metric` > then added new rule:

<img width="634" height="179" alt="image" src="https://github.com/user-attachments/assets/25dcb75b-7efd-4763-9397-34b89531be4d" />

* Metric source > `vmss1` aka current resource.
* Metric namespace > `virtual machine host`
* Metric name > `Percentage CPU`
* Operator > `Greater than`
* Metric threshold to trigger scale action > 70
* Duration > 10
* Time grain statistic > average
* Operation > increase percent by
* Cool down (minutes) > 5
* Percentage > 50

<img width="558" height="814" alt="image" src="https://github.com/user-attachments/assets/5172eb49-08d6-4de6-9070-fab518ae0b2d" />

Added and saved changes.

3. Lets say that in the evenings or weekends demand decreases, therefore its important to create a scale in rule. So next i created a rule that decreases the number of VM instances in scale set. 

**Important**

Number of instances should decrease when avg CPU load drops below 30% over 10 min period. When the rule triggers, then the number of V, instances is decreased by 20%.

4. Added new rule:

<img width="602" height="135" alt="image" src="https://github.com/user-attachments/assets/0191339a-81c9-4b61-a46d-4ccdc6ee3d5e" />

* Operator > less than
* Threshold > 30
* Operation > decrease percentage by
* Percentage > 50

<img width="561" height="401" alt="image" src="https://github.com/user-attachments/assets/1d46cb4a-6ec3-44a5-b778-5b444352ca67" />

Added and hit save.

5. Set the instance limits to make sure that i wouldnt scale out beyond maximum number of instances or scale in beyond the minimum number of instances.

<img width="523" height="215" alt="image" src="https://github.com/user-attachments/assets/7ce947df-c5ab-4636-8d3c-138ab53fc5a9" />

I couldnt save and got error:

<img width="337" height="261" alt="image" src="https://github.com/user-attachments/assets/0e109adc-e852-4459-ab0e-9fb494b2aadf" />

Turns out i hadnt registered `microsoft.insights` so anything that touches monitoring, metrics, diagnostics settings on the scale sets would fail until the provider would have been registered.

Went to `subscriptions` > selected mine > searched for `resource providers` then > `Microsoft.insights` and selected register at the top.

<img width="672" height="62" alt="image" src="https://github.com/user-attachments/assets/04a2f6df-5791-45a0-842f-77e6735ffa11" />

Tried updating in the other tab again and succeeded:

<img width="349" height="63" alt="image" src="https://github.com/user-attachments/assets/13855383-f57e-47a8-a817-62380c2e608e" />

## Part 5: Created VM using powershell

1. Opened up powershell in the portal top right ellipsis, then ran the following command:
```PWSH
New-AzVm `
-ResourceGroupName 'az104-rg8' `
-Name 'myPSVM'
-Location norwayeast `
-Image 'Win2019Datacenter' `
-Zone '1' `
-Size 'Standard_D2s_v3' `
-Credential (Get-Credential)
```

Output:

<img width="936" height="361" alt="image" src="https://github.com/user-attachments/assets/c8e69c6f-53d0-4123-bef3-728946112530" />

2. Used `Get-AzVM` to list VMs in my resource group:
```PWSH
Get-AzVM `
-ResourceGroupName 'az104-rg8' `
-Status
```

Output:

<img width="926" height="106" alt="image" src="https://github.com/user-attachments/assets/475baf8f-251c-456f-8e17-be03439b4c97" />

The VMs were running including the one i just created.

3. Used `Stop-AzVM` to deallocate the VM.
```PWSH
Stop-AzVM `
-ResourceGroupName 'az104-rg8' `
-Name 'myPSVM'
```

Output:

<img width="734" height="240" alt="image" src="https://github.com/user-attachments/assets/4ed9bc56-ef6c-49f1-b2e0-0d1a4b75e5f6" />

Part 6: Creating VM using CLI

1. Opened terminal and switched to `bash`. Then ran:
```BASH
 az vm create \
--name myCLIVM \
--resource-group az104-rg8 \
--image Ubuntu2204 \
--admin-username localadmin \
--generate-ssh-keys
```

But i hit a quoate wall in norway east region so switched it to westeurope:
```BASH
 az vm create \
--name myCLIVM \
--resource-group az104-rg8 \
--location westeurope \
--image Ubuntu2204 \
--admin-username localadmin \
--generate-ssh-keys
```

Then hit another wall with SKU: Standard_DS1_v2 not being available in that region, so added another line for Standard_B1s:
```BASH
az vm create \
--name myCLIVM \
--resource-group az104-rg8 \
--location westeurope \
--size Standard_B1s \
--image Ubuntu2204 \
--admin-username localadmin \
--generate-ssh-keys
```

And that worked:

<img width="943" height="468" alt="image" src="https://github.com/user-attachments/assets/a1e1eacf-07ec-490c-b515-0a36d0710794" />

2. Used `az vm show` to see details about the vm:
```BASH
az vm show \
--name  myCLIVM \
--resource-group az104-rg8 \
--show-details \
--output table
```

Output:

<img width="713" height="165" alt="image" src="https://github.com/user-attachments/assets/bb7cddeb-c64f-4710-9648-88aff77dba52" />

Then i used `az vm deallocate` to practice VM deallocation in bash:
```BASH
az vm deallocate \
--resource-group az104-rg8 \
--name myCLIVM
```

Then used `az vm show` again to make sure it was deallocated:
```BASH
az vm show \
--name  myCLIVM \
--resource-group az104-rg8 \
--show-details \
--output table
```

Output:

<img width="703" height="81" alt="image" src="https://github.com/user-attachments/assets/8a9dfa51-460b-4dd8-a579-9e682cba2378" />

Performed cleanup:
```BASH
az group delete \
--name az104-rg8
```

### Summary

### Key Takeaways
