# Lab 10: Implementing data protection

### Goal:

Learn how to backup and recover VMs. Learning how to create recovery service vault and backup policy for VMs as well as learning about disaster recovery with site recovery.
```ascii
REGION 1                            REGION 2
         [ az104-rg-region1 ]                [ az104-rg-region2 ]
      +-------------------------+         +-------------------------+
      |  +-------------------+  |         |  +-------------------+  |
      |  |      TASK 1       |  |         |  |      TASK 5       |  |
      |  |   az104-10-vm0    |--|-------->|  | az104-rsv-region2 |  |
      |  +-------------------+  | replication+-------------------+  |
      |                         |         +-------------------------+
      |  +-------------------+  |
      |  |      TASK 2       |  |
      |  | az104-rsv-region1 |  |
      |  +-------------------+  |
      |                         |
      |  +-------------------+  |
      |  |      TASK 3       |  |
      |  |   Azure Backup    |  |
      |  +-------------------+  |
      +------------^------------+
                   |
  TASK 4           |
 [ Monitor ] ------┘
 ```
 
**Job Skills**

* Using a template to provision infra.
* Creating and configuring recovery services vault.
* Configuring VM level backup.
* Monitoring Azure backup.
* Enabling VM replication.

## Part 1: Using template to provision an infra.

Downloaded lab 10 templates and parameters prior from their az 104 github repo prior to doing this lab.

1. Went to `deploy custom template` in the portal and loaded both template.json and parameter.json files then used resource visualiser to actually see what was going to be deployed:

<img width="866" height="731" alt="image" src="https://github.com/user-attachments/assets/e1dd1386-0a5d-42ea-9ac5-b200f2c0b1b9" />

Review before deployment:

<img width="496" height="531" alt="image" src="https://github.com/user-attachments/assets/e8658c09-29bf-4f28-b845-72e994a670ad" />

Hit create.

## Part 2: Creating and configuring recovery services vault

1. Went to `recovery services vault` and created new one.
2. In the `create recovery services vault` specified rg name `az104-rg-region1` and vault name `az104-rsv-region1`. For region used the norway east.

Hit create.

3. Went to the RSV resource > `settings` > `properties`.
4. Selected `update` > then `backup configuration` label. For storage replication left the `geo redundant` in place.
5. Selected `update` under `security setting` > `soft delete and security settings` label.
6. In the `security settings` noted that soft delete for workload was enabled and the period was set to 14 days.

<img width="813" height="131" alt="image" src="https://github.com/user-attachments/assets/a2168131-de2c-4a2f-a19a-18a28054bcd0" />

## Part 3: Configuring VM level backup

Needed to implement VM level backup and therefore define the backup and retention policy that applies to the backup. 

**Note**

Different VMs can have different backup and retention policies assigned to them.

1. In RSV overview selected `+ backup` and specified goals:

<img width="668" height="175" alt="image" src="https://github.com/user-attachments/assets/5ea2cfb5-301c-4671-8497-d205f03f7f10" />

2. Selected `backup`

Noticed there was 2 options, enhanced and standard:

Enhanced:

* Multiple backups per day
* Up to 30 days operational tier retention
* Support for trusted launch azure vm
* Support for VMs with Ultra Disks and Premium SSD v2

Standard:

* once a day backup
* Up to 5 days operational tier retention.

Since i wasnt doing anything long term i chose standard.

3. In the backup policy selected `create new policy` with following values:

* named it `az104-backup`
* Frequency > Daily
* Time > 12:00 AM
* Retain instant recovery snapshots for > 2 days

Hit ok

4. In the VM section hit add, then selected `az-104-10-vm0` hit ok and then enabled backup after reviewing settings:

<img width="938" height="877" alt="image" src="https://github.com/user-attachments/assets/550d9c14-8707-4395-9ceb-e8310d9faf68" />

5. After deployment went to resource, > `protected items` > `backup items` and selected `azure virtual machine` entry.
6. Selected `view details` link for `az104-10-vm0` to review values for backup pre check and last backup status entries:

<img width="300" height="89" alt="image" src="https://github.com/user-attachments/assets/8efbdf6a-58aa-4071-bd8f-b99e11662e3b" />

From this i could see that my backup was still pending because the first full snapshot hasnt completed. Backup pre check: passed meant that my backup was validated and was now ready to perform a snapshot and was waiting for data transfer.

7. Selected ellipsis at the end of the backup and chose `backup now` and accept default value in `retain backup till` then hit OK.

<img width="677" height="115" alt="image" src="https://github.com/user-attachments/assets/01257678-d70d-4231-9d19-b275aeee579a" />

## Part 4: Monitoring backup

1. Went to storage account and created new storage account.
2. Then searched for RSV again (recovery services vault) selected my vault and went to `Monitoring` > `diagnostic settings` > `add diagnostic setting`. Named it `Logs and metrics to storage`.
3. Checkmarked the following:

Log and metrics:

* Azure Backup Reporting Data > This centralizes historical backup metadata (protected items, storage usage, health trends) used to power the backup reports dashboard.
* Addon Azure Backup Job Data > Provides granular real time logs for individual backups and restores operations including start times, end times and specific failure codes.
* Addon Azure Backup Alert Data >  Captures official security and operational alerts triggered when critical backup failures or unauthorized deletions occur.
* Azure Site Recovery Jobs > Records execution details and status of replication tasks, failovers and test failover operations for DR strategy (disaster recovery). 
* Azure Site Recovery Events > Logs system level occurences and health changes within ASR (Azure site recovery fabric) fabric such as replication health degradation or configuration updates.

Then shecked `archive to a storage account` and selected my storage account `storagebackup900`

<img width="938" height="761" alt="image" src="https://github.com/user-attachments/assets/29053105-33fa-4caf-b257-a9c07b3bdf4e" />

Hit save.

4. Returned to recover services vaults then > `monitoring` > `backup jobs`

5. Towards the right chose `view details`:

<img width="892" height="509" alt="image" src="https://github.com/user-attachments/assets/8acf71b6-cf92-494a-90b2-a48128f5285b" />

## Part 5: Enabled VM replication

1. In the `recovery services vaults` created new one with following settings:

* created new resource group `az104-rg-region2`
* named the vault `az104-rg-region2`
* For region selected sweden central,  following region pair logic (because replicating into nearby region minimizes network latency which helps keep data synchronized in real time while also ensuring compliance with local laws) and since norway west was restricted for me.

<img width="775" height="583" alt="image" src="https://github.com/user-attachments/assets/df41dafa-d634-49ac-856c-d7818a831c6f" />

Hit create.

2. Went to my `az104-10-vm0` > `backup + disaster recovery` > `disaster recovery`. Saw that target region was selected east us so switched it sweden central since thats where i wanted data to be replicated.
3. In the `advanced settings` resource selections were made for me:

<img width="1263" height="782" alt="image" src="https://github.com/user-attachments/assets/85a75f27-c2ea-433d-999e-a604aa1b1975" />

Scrolled down and created automation account. Then reviewed settings:

<img width="662" height="810" alt="image" src="https://github.com/user-attachments/assets/ba0d8a1e-23de-407b-849d-2d9f517ff8fc" />

And started replication.

4. Replication took around 15 minutes total, then checked rsv-region2:

<img width="673" height="248" alt="image" src="https://github.com/user-attachments/assets/9740a5fd-747e-41e2-841b-e4aa9685c39f" />

Then checked the VM itself for details:

<img width="842" height="428" alt="image" src="https://github.com/user-attachments/assets/faa184f3-dfbc-4d37-87c8-74372e4d113a" />

Everything was healthy and no configuration issues were found, lab was complete and performed cleanup.

### Summary

Started by deploying the entire lab environment using an ARM template. Loaded the template and parameters into the custom deployment blade, reviewed the visualiser to understand what resources would be created, and deployed the infrastructure.

Created recovery services vault in region 1, configured its storage replication (G‑RS) and reviewed the security settings, including soft delete and retention defaults. This gave me the foundation for VM‑level backup.

Configured VM backup by creating a new backup policy, assigning it to the VM, and enabling protection. I reviewed the backup pre check status and triggered an on demand backup to start the first snapshot.

Set up monitoring by creating a storage account and enabling diagnostic settings on the vault. Selected all relevant backup and site recovery logs so that historical data, alerts, and job details would be archived for reporting and troubleshooting.

Created a second recovery services vault in region 2 to support disaster recovery. Configured VM replication from Region 1 to Region 2, selected Sweden Central as the target region, and let Azure generate the required resources. Waited for replication to complete and verified that the VM was healthy and fully protected.

Completed the lab by confirming backup status, replication health, and then performing cleanup by stopping backups in both regions and then soft deleting.

### What i learned

* Learned how ARM templates can quickly deploy consistent infrastructure.
* Learned how Recovery Services Vaults store backup and replication metadata.
* Learned how to create and apply VM‑level backup policies with custom retention.
* How to monitor backup jobs using diagnostic settings and storage accounts.
* How Azure Site Recovery replicates VMs across paired regions for disaster recovery.
* Learned how to validate replication health and understand the failover readiness workflow.

### Key Takeaways

* Azure Backup service provides simple, secure, and cost-effective solutions to back up and recover your data.
* Azure Backup can protect on-premises and cloud resources including virtual machines and file shares.
* Azure Backup policies configure the frequency of backups and the retention period for recovery points.
* Azure Site Recovery is a disaster recovery solution that provides protection for your virtual machines and applications.
* Azure Site Recovery replicates your workloads to a secondary site, and in the event of an outage or disaster, you can failover to the secondary site and resume operations with minimal downtime.
* A Recovery Services vault stores your backup data and minimizes management overhead.
