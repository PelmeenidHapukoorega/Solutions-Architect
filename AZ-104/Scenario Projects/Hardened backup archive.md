# Project Title: Hardened backup archive

Goal: A client needs a cloud-based location to store database backups. These backups contain sensitive data. The project must prioritize Security and Cost-Optimization.

## Requirements

1. Identity and storage

* Deploy a storage account using a redundancy level that protects against a single data center failure within one region.
* Disable all forms of anonymous public access.
* Ensure the storage account only accepts requests over secure connections.

2. Network security:

* The storage account must be "invisible" to the public internet.
* Configure the firewall so that no public networks can access it by default.

3. Governance and compliance

* Apply a tag to the resources identifying which department is paying for this.
* Implement a rule that automatically deletes files in the backup container after 14 days to keep costs low and satisfy data retention laws.

4. Data structure

* Create a container specifically for these backups.


