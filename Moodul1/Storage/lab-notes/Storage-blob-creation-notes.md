# Module: Azure storage services
## Lab: Azure Storage Account + Blob Access Control

### Objective
Deploy a storage account, create a private blob container, upload a file, test access behavior, and modify access levels.  
Focus is on understanding storage account configuration, blob security, and public access flows.

---

## 1. Create storage account

### Actions
1. Signed in to Azure Portal  
2. Created new RG `HermitBungalow`  
3. Created a **Storage Account** with the following configuration:

**Core settings**
* Storage account type: Azure Blob Storage or Azure Data Lake Storage Gen 2  
* Region: Default region  
* Redundancy: LRS
* Anonymous access on containers: Enabled (Advanced tab)

### Outcome
✔ Storage account successfully deployed  
✔ Unique global namespace assigned  
✔ LRS redundancy configured  
✔ Anonymous container access allowed (account level)

### Evidence
[![Resource Group](../screenshots/overview.PNG)](../screenshots/overview.PNG)

---

## 2. Create a Blob container

### Actions
1. Navigated to **Data storage → Containers**  
2. Created container with:  
   * Name: `<your-container>`  
   * Access level: Private (no anonymous access)

### Outcome
✔ Private container created  
✔ No public access allowed (default secure posture)

### Evidence
[![Resource Group](../screenshots/basement.PNG)](../screenshots/basement.PNG)

---

## 3. Upload a blob

### Actions
1. Downloaded or selected an image file locally  
2. Uploaded file into the container  
3. Opened blob → viewed its properties  
4. Copied blob URL  

### Verification  
Attempted to open blob URL in a new browser tab.  
Result: Access denied because container is private.

Expected error:
`../screenshots/errorohno.png`
### Outcome
✔ Blob uploaded 
✔ URL reachable but blocked due to private access level
