# Azure Identity, Access, & Security

![Azure](https://img.shields.io/badge/azure-%230072C6.svg?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Status](https://img.shields.io/badge/Status-Learning_Path-yellow?style=for-the-badge)

> **Architectural Philosophy:** Identity is the new firewall. If you want to secure anything in Azure, you treat identity like the front door to your kingdom. Permissions, policies, and verifications lock together like a vault puzzle.

> **Bottom line:** Lock your house now, because Future You isn't trying to explain to your boss why some rogue Service Principal just joy-rode through your subscription like it's GTA Free Roam.

## 📚 Table of Contents
- [1. Azure Directory Services](#1-azure-directory-services)
- [2. Authentication Methods](#2-authentication-methods)
- [3. External Identities](#3-external-identities)
- [4. Conditional Access](#4-conditional-access)
- [5. Role-Based Access Control (RBAC)](#5-role-based-access-control-rbac)
- [6. Zero Trust Model](#6-zero-trust-model)
- [7. Defense in Depth](#7-defense-in-depth)
- [8. Microsoft Defender for Cloud](#8-microsoft-defender-for-cloud)

---

## 1. Azure Directory Services

**Microsoft Entra ID** (formerly Azure AD) is the identity backbone. It handles authentication, SSO, and device management. If the account isn't right, nothing else works right.

### The Ecosystem
1.  **Microsoft Entra ID:** The cloud boss. Handles global auth, SSO, and App Management.
2.  **Entra Connect:** The bridge. Syncs on-prem AD to the cloud.
3.  **Entra Domain Services:** The legacy compatibility layer. Managed Domain Controllers for apps that need Kerberos/NTLM/LDAP.

### Identity Sync Flow
How on-prem and cloud identities merge:

```mermaid
graph TD
    OnPrem[🏢 On-Premises Active Directory]
    Connect[🔁 Entra Connect Sync]
    Entra[☁️ Microsoft Entra ID]
    CloudApps[📱 Cloud Apps / SaaS]
    DS[📦 Entra Domain Services]
    Legacy[📠 Legacy Workloads via LDAP/Kerberos]

    OnPrem -->|Syncs Identities| Connect
    Connect -->|Populates| Entra
    Entra -->|Auth Token| CloudApps
    Entra -.->|One-Way Sync| DS
    DS -->|Domain Join/Auth| Legacy
