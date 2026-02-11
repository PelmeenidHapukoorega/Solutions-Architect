# Notes on determining NSG rules

* Security rules in NSG enable network traffic filtering.
* Defining rules helps control traffic flow in and out of VNet subnets and NICs.

## Things to know about security rules

Characteristics of security rules in NSG

* AZ creates several security rules within each NSG, including inbound and outbound traffic.
  * Example: defaule rules `DenyAllInbound` traffic and `AllowInternetOutbound` traffic.

* AZ creates default sec rules in each NSG created.

* More sec rules can be added to NSG by specifying conditions.

List of most common conditions:

<img width="686" height="358" alt="image" src="https://github.com/user-attachments/assets/b28cf108-f0e8-4066-b717-f3e8ee7f14ec" />

* Each sec rule is assigne `Priority value`
  * All sec rules for NSG are processed in priority order.
  * When rule has low Priority value, the rule has a higher priority or precedence in terms of order processing.

* Default security rules cant be removed.

* Defaul sec rules can be overridden by creating another sec rule that has a higher priority setting for NSG.

## Inbound traffic rules

* AZ defines 3 default inbound sec rules for NSG.
* These rules `deny all inbound traffic` except traffic from VNet and load balancers.

## Outbound traffic rules

* AZ defines 3 default outbount sec rules for NSG
* These rules `only allow outbound traffic` to internet and VNet.
