# Cloud shell 

# Cloud Shell Runbook

**Table of Contents**
1. [The Concept](#1-the-concept)
2. [Architecture & Constraints (For Exam prep)](#2-architecture--constraints-for-exam-prep)
3. [Operations (CLI & PowerShell)](#3-operations-cli--powershell)
4. [Troubleshooting & Labs (Break-Fix)](#4-troubleshooting--labs-break-fix)
5. [Advanced Mechanics (The "Gotchas")](#5-advanced-mechanics-the-gotchas)
6. [Lifecycle & Management (The Missing Pieces)](#6-lifecycle--management-the-missing-pieces)
7. [Lab Workflow (Getting External Files)](#7-lab-workflow-getting-external-files)
8. [Productivity & Speed Hacks (Lab Accelerators)](#8-productivity--speed-hacks-lab-accelerators)

---
## 1. The Concept
*   **What is it?** An interactive, authenticated, browser-accessible shell for managing Azure resources.
*   **Why use it?** No installation required. It maintains your credentials so you don't have to log in repeatedly. It has common tools pre-installed (Terraform, Ansible, Az CLI, PowerShell).
*   **Security:** Compliant by default with double encryption at rest.
*   **Access Methods:** Portal icon, MS Learn snippets, or the direct URL: https://shell.azure.com.

## 2. Architecture & Constraints (For Exam prep)
*   **Storage Requirement:** Cloud Shell **requires** an Azure Storage Account (Files share) to persist your `$HOME` directory.
    *   *Note:* You can run "Ephemeral" mode (no storage), but you lose all scripts when you close the window.
*   **Timeout:** The shell times out after **20 minutes** of inactivity. You cannot increase this.
*   **Networking:** Cloud Shell runs in a container managed by Microsoft. To access resources inside a Private VNET, you must configure "Cloud Shell in a VNET" (Advanced topic).
*   **Editor:** Contains a built-in code editor (Monaco). Open it by typing `code .`.
*   **No Root Access (sudo):** You are a standard user. You cannot run commands requiring admin permissions (like sudo apt-get install). You must install tools to your local user scope.
*   **Concurrency Limit:** You generally cannot open multiple independent sessions simultaneously; it is designed for one active instance per user.
*   **Long-Running Scripts:** Because of the 20-minute timeout, Cloud Shell is not suitable for long background tasks. If the connection drops, the state is lost.

## 3. Operations (CLI & PowerShell)

### Essential Commands

| Action | Azure CLI (Bash) | PowerShell |
| :--- | :--- | :--- |
| **Switch Subscription** | `az account set -s "SubName"` | `Select-AzSubscription -Name "SubName"` |
| **List Resources** | `az group list -o table` | `Get-AzResourceGroup \| Format-Table` |
| **Get Help** | `az vm create --help` | `Get-Help New-AzVM -Examples` |
| **Upload/Download** | Use the GUI icon in the toolbar | Use the GUI icon in the toolbar |

### The "Cloud Drive"
*   Files saved in `$HOME` (Bash) or `$Home` (PS) persist.
*   Files saved outside (e.g., `/usr/bin`) are **deleted** when the session ends.
*   **Accessing files:** Your storage account is mounted as `clouddrive`.

**Bash:**
```bash
cd clouddrive
```

* **Pre-installed Tool Categories:**
  * **Containers:** Docker, Kubectl, Helm.
  * **Databases:** MySQL client, PostgreSQL client, sqlcmd.
  * **Infrastructure:** Terraform, Ansible, Chef, Puppet.

## 4. Troubleshooting & Labs (Break-Fix)

### Issue: "Storage Account Creation Failed"
*   **Scenario:** First time launch, you selected "Create Storage," but it failed.
*   **Cause:** You might not have permissions to create Storage Accounts, or the generated name was not unique globally.
*   **Fix:** Click "Show Advanced Settings" during setup and manually select an existing Resource Group and unique Storage Account name.

### Issue: "Stuck at Requesting a Cloud Shell"
*   **Cause:** WebSocket connection blocked by corporate firewall or browser ad-blocker.
*   **Fix:** Disable ad-blockers for `portal.azure.com` or try a different network.

### Issue: "I lost my installed modules"
*   **Scenario:** You installed a custom PowerShell module, closed the browser, and it's gone.
*   **Cause:** You installed it in the container system folders, not your home drive.
*   **Fix:** Always install modules to your current user scope:
    ```powershell
    Install-Module -Name X -Scope CurrentUser
    ```
    
## 5. Advanced Mechanics (The "Gotchas")

### The "5GB Home Directory Trap"
*   **The Architecture:** Cloud Shell mounts your Azure File Share to `clouddrive` (5TB+ space), but your user home folder (`$HOME` or `~`) is a temporary 5GB disk image.
*   **The Risk:** If you install heavy PowerShell modules or Python libraries directly to `~`, you will hit the 5GB limit and the shell will stop starting.
*   **The Fix:** Always save large files/scripts in `clouddrive`, not the root of your user folder.

### VNET Integration (Private Access)
*   **Scenario:** You need to manage a VM that has **no Public IP** using Cloud Shell.
*   **Required Resources:** To make this work, you must deploy:
    1.  **A Dedicated Subnet:** Cloud Shell containers need their own subnet.
    2.  **Azure Relay:** This bridges the connection between your browser (Public Internet) and the Private VNET.
    3.  **Resource Providers:** You must register `Microsoft.ContainerInstance` and `Microsoft.Relay` in your subscription.

### Web Preview
*   **Use Case:** You write a Python script that runs a web server on port 8080.
*   **How to view it:** You cannot access `localhost:8080` directly.
*   **The Command:** Click the "Web Preview" icon in the Cloud Shell toolbar > "Configure" > Port 8080 > "Open". Azure opens a proxy URL for you.

## 6. Lifecycle & Management (The Missing Pieces)

### How to Reset / Change Storage Account
*   **The Scenario:** You clicked "Create" too fast during setup, and it made a generic storage account in the wrong region. You need to switch to your correct `storage-lab-01`.
*   **The Fix (Bash):**
    1. Run: `clouddrive unmount`
    2. Confirm: `y`
    3. Refresh the page. You will be prompted to run the setup wizard again.
*   **The Fix (PowerShell):**
    1. Run: `Dismount-CloudDrive`
    2. Refresh the page.

### Persistence (What dies vs. What stays)
*   **Files:** Files in `$HOME` and `clouddrive` stay.
*   **Tools:** If you run `sudo apt-get install nmap`, it works for *this session only*. Once you close the tab, the container is destroyed. You must reinstall custom tools every time (or use a setup script saved in your `clouddrive`).

### Cost & Availability
*   **The Shell:** Free (Compute is provided by Microsoft).
*   **The Cost:** You pay for the **Storage Account** (Files Share) and **Data Transfer** (if VNET integration is used).
*   **Availability:** Cloud Shell is not available in every single region. It acts as a regional service. Your storage account generally should be in the same region as the Shell mount to reduce latency.

## 7. Lab Workflow (Getting External Files)

### The "Git Clone" Method (Crucial for Labs)
*   **The Scenario:** You need to run Microsoft's official lab scripts (ARM templates/Bicep) found on GitHub.
*   **The Workflow:**
    1. Find the GitHub URL of the lab repo.
    2. Run in Cloud Shell:
       ```bash
       git clone https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator
       ```
    3. This downloads the entire folder structure into your `$HOME` directory.
    4. Navigate to the specific lab file:
       ```bash
       cd AZ-104-MicrosoftAzureAdministrator/Lab01
       ```

### Switching Environments
*   **The Dropdown:** You can switch between Bash and PowerShell using the dropdown menu in the top left of the terminal window.
*   **The Effect:** This creates a **new** session. Variables defined in Bash do **not** carry over to PowerShell, but files in `clouddrive` are shared between both.

## 8. Productivity & Speed Hacks (Lab Accelerators)

### The "Interactive" Mode (The Cheat Sheet)
*   **The Problem:** You forget the exact syntax of a command (e.g., was it `--resource-group` or `-g`?).
*   **The Hack:** Type `az interactive` in the Cloud Shell.
*   **The Result:** It turns the shell into an auto-complete environment. As you type `az vm`, it lists every possible next command and flag instantly. It also provides examples at the bottom of the screen.
*   **To Exit:** Type `exit`.

### The Editor Shortcuts (Monaco)
*   **Context:** When using `code .`, you don't have a "File > Save" menu easily accessible with a mouse.
*   **Save:** `Ctrl + S` (Windows) / `Cmd + S` (Mac).
*   **Open Command Palette:** `F1`.
*   **Close Editor:** `Ctrl + Q`.

### Drag & Drop Upload
*   **The Hack:** You don't need to click the "Upload" button. You can drag a script file from your local desktop directly into the black terminal window of Cloud Shell. It will upload automatically to your `$HOME` folder.
