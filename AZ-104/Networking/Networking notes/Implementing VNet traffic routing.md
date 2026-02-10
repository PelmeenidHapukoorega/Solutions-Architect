# Notes about implementing VNet traffic routing

* Azure creates automatically route table for each subnet within VNet.
* Route table has default system routes and any user defined routes required.

## System routes

* System routes are automatically created and assigned to each subnet within the VNet.
* System routes cant be created or removed but some system routes can be overrided with custom routes.
* AZ creates default system routes for each subnet and adds other optional default routes to specific subnets or every subnet when using specific AZ capabilities.
