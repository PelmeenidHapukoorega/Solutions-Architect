# Notes about configuring public IP services

* Public networks like the internet communicate by Public IP addresses.
* Private networks like VNets use Private IP addresses which arent routable on public networks.

To support a network that exists both in Azure and On-Premises means that IP addressing needs to be configured for both types.

* Public IP addresses enable Internet resources to communicate with Azure resources.
* Enable resources to communicate outbound with internet and public-facing Azure services.
* Public IP address is dedicated to a specific resource.
* Resource without public IP assigned can communicate outbound through network address translation services where Azure dynamically assigns an available IP address that isnt dedicated to the resource.
