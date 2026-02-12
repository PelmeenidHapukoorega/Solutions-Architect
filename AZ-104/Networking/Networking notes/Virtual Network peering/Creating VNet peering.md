# Notes about creating VNet peering

1. Required permissions

* Role: Your account must have the Network Contributor role.
* Custom Roles: If not a Network Contributor, you must have the specific permission `Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write`.

2. The 2 way street requirement

* Peering is not automatic for both sides. To successfully connect VNet-A and VNet-B:
  * You must create a peering link from A to B.
  * You must create a peering link from B to A.
* Communication will fail until both links are created and active.

3. Understanding peering status

Most important part to monitor in the portal:

* **Initiated** > You created the link on one VNet, but the other side hasnt been linked yet. `Communication` > NO
* **Connected** > Both VNets have links pointing to each other. `Communication` > YES
* **Disconnected** > One side was deleted or the link was manually broken. `Communication` > NO

4. Technical terminology

* Local Network: The VNet you are currently configuring.
* Remote Network: The "second" VNet you are trying to connect to.
* Address Space: You cannot change the address space of a VNet while it is peered (in some regions/configurations). It is best practice to finalize your IP ranges before peering.

5. VM communication behavior

* Default: Before peering, VMs are isolated.
* After Peering: VMs can communicate using Private IPs.
* Note: Peering does not automatically grant access: Network Security Groups (NSGs) can still block traffic if rules are not set to "Allow."

6. Tip for notes

**The "Synchronous" Creation:** In the modern Azure Portal, when you add a peering to VNet-A, there is a checkbox/section that allows you to create the reciprocal peering on VNet-B at the same time.

* Always use this option to ensure both sides move to Connected status instantly.
* If you dont use this, you have to manually go to the second VNet and "complete" the peering.
