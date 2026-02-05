# Lab 8: Managing virtual machines

## Goal:

Create and compare VMs to VM scale sets. Learning how to configure and resize single VM, how to create scale sets and configure autoscaling. 
```Mermaid
graph TD
    subgraph Task_1 [Task 1]
        direction LR
        VM1["az104-vm1<br/>Zone1"]
        VM2["az104-vm2<br/>Zone2"]
    end

    subgraph Task_2 [Task 2]
        T2_Action["💻 💾<br/>resize and update"]
    end

    VM1 --> Task_2
```

**Job Skills**

* Deploying zone resilient VMs using portal.
* Managing compute and storage scaling for VMs.
* Creating and configuring VM scale sets.
* Scaling VM scale sets.
* Creating VM using PWSH.
* Creating VM using CLI.

## Part 1: Creating and configuring storage account
