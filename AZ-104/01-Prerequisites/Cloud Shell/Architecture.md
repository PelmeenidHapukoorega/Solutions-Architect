## Architecture of Cloud Shell

Rules to follow:

1. SKUs & Tiers: Compare features of different levels (basic vs standard load balancer)
2. Dependencies: List what must exist before you create a resorces (VPN gateway requires "gatewaysubnet" precisely named)
3. SLA & Limits: What are the hard limits? (Max 10 VNet peerings per VNet)
4. Decision Matrix: Small table: "Use service A if you need X; use service B if you need Y logic"
