# Notes about identifying routing capabilities in a VNet

1. System routes (defaults)

* Automatic: Azure creates these for every subnet by default. You cannot create or delete them.
* Connectivity: By default, all subnets in a VNet can communicate with each other, and all can reach the Internet (0.0.0.0/0).
* The "None" Hop: Azure automatically creates "None" routes for private IP ranges (like 10.0.0.0/8). This means traffic to these ranges is dropped unless they are part of your VNet address space.
