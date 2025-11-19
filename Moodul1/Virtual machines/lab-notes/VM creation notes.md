# Module: Core architectural components of Azure
# Virtual machine creation (Azure Portal) — Lab Notes

To actually understand Azure IaaS you gotta know how the bread and butter VM gets deployed.  
This is your entry point into workloads, networking, storage, identities — everything snaps around the VM later.  
**Bottom line:** If you can build VMs with confidence, you can build the whole foundation later.


## Steps I took

1. **Created Azure trial account**  
   Fresh subscription shows up as *Azure subscription 1* if using a trial.

2. **Opened the Virtual Machines service**  
   From Azure Home → “Virtual machines”.

3. **Started deployment**  
   Top menu → **+ Create** → **Azure virtual machine**

4. **Entered required values (from learning path)**  
   **Subscription:** Azure subscription 1  
   **Resource Group:** Lab suggested *IntroAzureRG*, I used **Virtukas**  
   **VM Name:** **my-virtukas**  
   **Region:** Left default (**Norway East**)  
   **SSH Settings:** Allowed SSH from internet (lab scenario only)  
   **Username:** **azurehermit**  
   **Rest:** Left defaults (size, disks, networking, management)

5. **Reviewed & deployed**  
   Hit **Review + create**  
   Validation took ~30 sec  
   Hit **Create**  
   Deployment finished in ~20 sec

6. **Verified the resource**  
   Home → **Resource Groups** → opened **Virtukas**  
   VM appeared correctly inside the RG.

7. **Screenshots**  
   Provided in:  
   `Module1-VM/` folder inside repo  

## What I learned

**Key points**
* Portal-based VM deployment is the simplest way to create IaaS resources  
* Resource Group organizes the VM and all its related assets  
* Region affects cost, latency, availability  
* SSH authentication chosen at creation time becomes permanent unless reconfigured  
* Defaults work for basic labs but are never recommended for production

## Verification Evidence
* VM successfully created  
* Resource Group visible with all related components  
* Deployment history in portal confirms steps  
* Screenshots stored in repo

## Takeaway
If you can deploy a VM, you’ve officially unlocked the base layer of Azure IaaS.  
Every future workload (networking, storage, security, automation) builds on this foundation.  

Congrats — VM deployed like a champ.

## References
https://learn.microsoft.com/en-us/training/modules/describe-core-architectural-components-of-azure/
