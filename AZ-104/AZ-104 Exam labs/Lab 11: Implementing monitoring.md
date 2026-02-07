# Lab 11: Implementing monitoring

### Goal:

Learn how to backup and recover VMs. Learning how to create recovery service vault and backup policy for VMs as well as learning about disaster recovery with site recovery.

```ascii
_________________________________________________________________________
| [ az104-rg11 ]                                                        |
|                                                                       |
|  +-------------------+        +------------------------------------+  |
|  |      TASK 1       |        |               TASK 2               |  |
|  |   az104-11-vm0    |---┐    |   Alert - delete virtual machine   |  |
|  +-------------------+   |    +------------------------------------+  |
|                          |                                            |
|                          |    +------------------------------------+  |
|                          |    |               TASK 3               |  |
|                          |    |        Action - send email         |  |
|                          |    +------------------------------------+  |
|                          |                                            |
|  +-------------------+   |    +------------------------------------+  |
|  |      TASK 6       |   |    |               TASK 4               |  |
|  |    Log Queries    |   └--->|         Trigger the alert          |  |
|  +-------------------+        +------------------------------------+  |
|                                                                       |
|                               +------------------------------------+  |
|                               |               TASK 5               |  |
|                               |       Add a processing rule        |  |
|                               +------------------------------------+  |
|_______________________________________________________________________|
 ```

**Job Skills**

* Using template to provision infra
* Creating alerts
* Configuring action group notifications
* Triggering an alert and confirming that it functions
* Configuring alert processing rule
* Using monitor log queries

1. Created new resource group `az104-rg11`. Then in `deploy a custome template` uploaded the template.json file i had downloaded from the official repo.
