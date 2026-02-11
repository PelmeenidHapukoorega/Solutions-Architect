# Note about implementing NSG

NSG is used to limit network traffic in a VNet. You can assign it to a subnet or network interface as well as define rules in the group to control network traffic.

## Things to know about NSG

* NSG contains list of security rules that allow or deny inbound or outbound network traffic.
* NSG can be associated to a subnet or network interface.
* NSG can be associated multiple times.
* In the overview page you can see details about assigned subnets, network interfaces and defined security rules.


## NSG and subnets

* NSG assigned to a subnet creates protected screened subnet (DMZ).
* DMZ acts as a buffer between resources within VNet and the internet.

* Use NSG to restrict traffic flow to all machines that reside in the subnet.
* Each subnet can have maximum of 1 associated NSG.

## NSG and network interfaces

NSG can be assigned to NICs

* Define NSG rules to control traffic that flows through NIC
* Each NIC that exists in a subnet can have 0 or 1 associated NSG.
