```ascii
+---------------------------------------------------------------+
|                  SHARED RESPONSIBILITY MODEL                 |
+--------------------------+------------------------------------+
| Cloud Provider (Azure)   | Customer (You / Business)          |
+--------------------------+------------------------------------+
| Physical Datacenter      | Data                               |
| - Buildings              | - Files, databases, backups        |
| - Power, cooling         |                                    |
| - Physical security      |                                    |
+--------------------------+------------------------------------+
| Hardware & Infrastructure| Identities & Access                |
| - Servers                | - Users, roles, MFA                |
| - Storage devices        | - Permissions / RBAC               |
| - Networking equipment   |                                    |
+--------------------------+------------------------------------+
| Host OS & Platform       | Application Security               |
| (depends on service type)| - Code, configs, updates           |
|                          | - Vulnerabilities                  |
+--------------------------+------------------------------------+
| Network Security         | Resource Configuration             |
| - Firewalls              | - NSGs, subnet rules               |
| - DDOS protection        | - VM setup, storage settings       |
+--------------------------+------------------------------------+
| Service Maintenance      | Operational Policies               |
| - Patching host OS       | - Monitoring                       |
| - Updating platform      | - Logging / alerting               |
+--------------------------+------------------------------------+
| SaaS → Provider handles most things                           |
| PaaS → Shared control depending on layer                      |
| IaaS → Customer handles most things                           |
+---------------------------------------------------------------+
```

```kotlin
In IaaS → Customer manages more (VMs, OS, configs).
In PaaS → Customer manages app + data, Azure manages platform.
In SaaS → Customer only manages data & access, Azure manages everything else.
```
