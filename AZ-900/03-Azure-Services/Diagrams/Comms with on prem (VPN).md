```ascii
 On-prem network                 Azure
 192.168.0.0/16                  10.0.0.0/16
+-------------+            +----------------+
| On-prem FW  |============| VPN Gateway    |
| Router      |   IPSec    | (GatewaySubnet)|
+-------------+  tunnel    +----------------+
        |                          |
   +---------+                +---------+
   | Servers |                |  VMs    |
   +---------+                +---------+

Legend: "============" = encrypted VPN tunnel
```
