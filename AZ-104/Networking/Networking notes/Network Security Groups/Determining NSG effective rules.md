# Notes about determining NSG effective rules

Things to consider when creating effective rules:

1. Processing order

Traffic is evaluated in a specific sequence depending on the direction. If you have NSGs at both the Subnet and the NIC level, it works like this:

* Inbound Traffic: Subnet NSG first >  NIC NSG second. (The traffic must pass the "gate" of the subnet before it can reach the specific VM).
* Outbound Traffic: NIC NSG first >  Subnet NSG second. (The traffic must leave the VM "gate" before it can exit the subnet).


2. Both must agree rule

* For traffic to successfully reach its destination, both the Subnet NSG and the NIC NSG must have an Allow rule.
* If either level has a Deny rule that matches the traffic, the traffic is blocked.

3. Rule priority logic

* Rules are processed in Priority Order: The lower the number, the higher the priority.
* Once a rule matches the traffic, processing stops.
* Best Practice: Use gaps in numbering (100, 200, 300). This allows you to "sandwich" a new rule in between (like 150) later without renumbering everything.

4. Intra-subnet traffic (VM-to-VM)

* NSG rules arent just for traffic coming from the internet, they also apply to traffic between VMs in the same subnet.
* If you want to stop two VMs in the same subnet from talking to each other, you must explicitly create a Deny rule in the NSG associated with that subnet.

5. Default behavior (no NSG)

* If you dont associate an NSG with a subnet or NIC, AZ doesnt block everything. Instead it follows Default Azure Security Rules which generally allow all traffic within the virtual network and all outbound traffic to the internet.

6. Troubleshooting tool

* **Effective Security Rules:** If you are ever confused about why a VM cant connect, go to the VM in the portal and look for the Effective security rules link.
* This tool does the math for you, combining the Subnet and NIC rules into one final list so you can see exactly which rule is causing the "Deny."


Summary cheat sheet:

* Inbound: Think of it as entering a building: you pass the Subnet (Main Gate) then the NIC (Room Door).
* Outbound: Think of it as leaving: you pass the NIC (Room Door) then the Subnet (Main Gate).
* Priority: The lower the number, the more powerful the rule. 100 beats 65000 every time.
