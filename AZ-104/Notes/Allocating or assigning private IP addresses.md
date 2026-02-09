# Notes about allocating or assigning private IP addresses

Private IP adress resource can be associated with:

* VM nework interfaces
* Internal load balancers
* Application gateways

Azure can provide IP address (dynamic assignment) or you can assign the IP address (static assignment).

## Things to consider when associating private IP addresses

Table summary on how to assign private IP addresses for different resources:

<img width="700" height="191" alt="image" src="https://github.com/user-attachments/assets/a1ccf324-0f13-46dc-89c4-a089dc04dd69" />

## Private IP assignment

Private IP address is allocated from the address range of the virtual network subnet that a resource is deployed in. There are 2 options: Dynamic and Static.
