# Project 02: Secure Data Tier and Automated Lifecycle

### Primary infrastructure:

**GOAL:**

Designing a multi-layered storage architecture for a medical provider that ensures 100% network isolation, automated cost-optimization, and granular data governance.

* **Data Lake (ADLS Gen2):** Centralized repository for medical imaging and patient records using Hierarchical Namespace (HNS) for high-performance file-level operations.
* **Legacy Integration Layer:** Enabled SFTP support on the primary storage account to support legacy diagnostic equipment that cannot use modern REST APIs.
* **Shared Application Configs:** Deployed a Premium Azure File Share to host shared configuration files for web servers across multiple regions.

### Security and inspection:

* **Private Link Integration:** All public internet access is disabled. Storage is accessible only via Private Endpoints within a restricted Management VNet.
* **Immutable Storage:** Configured "Time-based retention" policies on the Archive container to meet legal compliance, preventing any data deletion or modification for 7 years.
* **SAS Governance:** Implemented User Delegation SAS tokens, ensuring that temporary access to external partners is tied to Entra ID identities rather than static Account Keys.

### Data lifecycle and optimization

* **Automated Tiering:** Implemented a Lifecycle Management policy to automatically transition data:
  * Hot to Cool: After 30 days of no modification (Imaging data).
  * Cool to Archive: After 180 days (Historical patient records).
  * Cleanup: Automated deletion of temporary staging snapshots after 7 days.
* **Geo-Redundancy:** Configured GZRS (Geo-Zone-Redundant Storage) to ensure data persists even if an entire Azure Region suffers a failure.

## Management and resilience

* **Zero-Key Access:** All management tasks and application access are handled via System-Assigned Managed Identities and RBAC (Storage Blob Data Contributor).
* **Soft Delete:** Enabled "Soft Delete" for both Blobs and File Shares with a 14-day retention period to recover from accidental administrative errors.
* **Monitoring:** Diagnostic settings configured to stream "StorageRead", "StorageWrite", and "StorageDelete" logs to a Log Analytics Workspace for audit auditing.
