# Notes about configuring internet access with Virtual NAT

* Globally IPv4 are in short supply and can be expensive way to grannt access to internet resources.
* AZ NAT (network address translation) lets internal resources on a privated network to share routable IPv4 addresses.
* NAT service maps outgoing requests from internal resources to an external IP address.

Diagram of outbound traffic flow from S1 through NAT gateway to be mapped to a public IP address or a punlic IP prefix:
```ascii
(   Internet   )
               Outbound |    Return    | Inbound
                  ^     |      :       |    |
                [OK]    |    [OK]      |   [X]
                  |     |      v       |    v
                  +-----+------+-------+----+
                        |  Public IP   |
                        +------+-------+
                               |
                        [ NAT Gateway ]
                               |
        _______________________|_______________________
        | Virtual Network      |                      |
        |                      |                      |
        |  +----------------+  |  +----------------+  |
        |  |    Subnet 1    |  |  |    Subnet 2    |  |
        |  |  [VM]    [VM]  |  |  |  [VM]    [VM]  |  |
        |  +----------------+  |  +----------------+  |
        |______________________|______________________|
```

* When NAT is configured all UDP and TCP outbound flows from any VM instance will use NAT for internet connectivity.
  * No further configuration is necessary.
  * No need to create any UDR.
  * NAT takes precedence over other outbound scenarios and replaces default internet destination of a subnet.

* NAT scales automatically to support dynamic workloads.
* Nat supports up to 16 public IP addresses.
* Using port network translation (PNAT or PAT), NAT provides 64 000 concurrent flows for UDP and TCP.
* NAT is compatible with following standard SKUs:
  * Load balancer
  * Public IP address
  * Public IP prefix

## NAT limitations

* Only IPv4 address family is supported. NAT doesnt interact with IPv6 at all.
* NAT cant spin multiple VNets.
* IP fragmentation is not supported
