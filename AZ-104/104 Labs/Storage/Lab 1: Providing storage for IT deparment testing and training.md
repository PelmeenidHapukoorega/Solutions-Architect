# Lab 1: Providing storage for IT deparment testing and training

1. Created resource group `StorageOnly`. Used norwayeast as region.
2. Went to `storage accounts` landing page > `+ create` and filled out the basics:

<img width="807" height="624" alt="image" src="https://github.com/user-attachments/assets/ed1a1326-d5c4-4c9f-9d75-b116799fd26e" />

Hit create.

3. Went to `data management` blade and found out i could change redundancy at will in there. Although i had it configured before creation for the redundancy to be LRS whis is local redundancy and cheaper.

Checked under `Settings` > `Configuration` to see if `Secure transfer required` was enabled. When enabled it allows requests to the storage account only via secure connection.

Checked the same page for `Minimal TLS version` to be set at version 1.2. TLS is a security protocol designed to facilitate privacy and data security for communications over the internet.

Ensured that `Allow storage account key access` was in fact disabled. If this option is allowed then storage account key access is disabled, any requests to the account that are authorized with shared key, including shared access signatures (SAS), would be denied.

Checked under `Security + networking` > `networking` to make sure `Public network access` was enabled from all networks. This allowed me to test my connection with ease however in real life practice this is considered a security risk because it leaves my storage endpoint exposed to the public internet.

### Summary

### Key takeaways

* Storage account is a container that holds all azure storage data objects including blobs, files, queue and tables.
* Storage offers several types of storage accounts, standard and premium. Each supports different features and has its own pricing model.
* Storage always stores multiple copies of your data to protect it from planned and unplanned events.
* Redundancy models can replicate data in the primary and secondary regions.
