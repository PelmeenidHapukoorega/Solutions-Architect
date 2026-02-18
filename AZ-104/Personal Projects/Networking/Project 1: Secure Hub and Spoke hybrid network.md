# Project: Secure Hub-and-Spoke Hybrid network

## Architecture scenario

Scenario: Medical company is migrating its legacy patient management system to Azure.

They have strict compliance requirement: All traffic leaving the application environments must be inspected by a central security appliance. In additon their `on-premises` staff needs to access AZ resouces securely as if they were on the local network.

* **Architecture:** Implementing Hub and Spoke topology
  * Hub: Has to contain shared infra (security and connectivity)
  * Spokes: 2 Seperate VNets (Pharmacy and Research)

* **On prem simulation:** 4th VNet in a different region. Establishing `site-to-site VPN` between the mock on prem VNet and the Hub to simulate hybrid connection.

* **Security and inspection:**
  * Deploying AZ FW in the hub
  * Configuring UDRs so traffic between Pharmacy and Research spokes is forced through the FW in the hub (Inter-spoke inspection)
  * Directing all internet-bound traffic from the spokes through the firewall

* **Application delivery:**
  * Pharmacy portal (simulated by a VM) has to be accessible from the public internet, but only via Application gateway with WAF enabled

* **Management:**
  * No VMs should have public IP addresses
  * Access for troubleshooting has to be handled via AZ Bastion deployed in the Hub

* **Identity and naming:**
  * Using ASGs to manage traffic rules for the web servers in the Pharmacy spoke, rather than relying solely on individual IP addresses in the NSGs

## Resource groups and VNets 

1. Created new resource group where i would hold the companys VNets
```bash
az group create --name MedCenter --location norwayeast
```

Used `az group list` to verify its existance:

<img width="807" height="499" alt="image" src="https://github.com/user-attachments/assets/532bd9ab-0722-4608-8de5-1db8123e5891" />

2. Created `Hub-Vnet` and added subnets to it:
```bash
az network vnet create \
--name Hub-VNet \
--resource-group MedCenter \
--address-prefixes 10.0.0.0/16
```

AzureFirewallSubnet:
```bash
az network vnet subnet create \
--name AzureFirewallSubnet \
--resource-group MedCenter \
--vnet-name Hub-VNet \
--address-prefixes 10.0.1.0/24
```

GatewaySubnet:
```bash
az network vnet subnet create \
--name GatewaySubnet \
--resource-group MedCenter \
--vnet-name Hub-VNet \
--address-prefixes 10.0.2.0/24
```

AzureBastionSubnet:
```bash
az network vnet subnet create \
--name AzureBastionSubnet \
--resource-group MedCenter \
--vnet-name Hub-VNet \
--address-prefixes 10.0.3.0/24
```

AppGatewaySubnet:
```bash
az network vnet subnet create \
--name AppGatewaySubnet \
--resource-group MedCenter \
--vnet-name Hub-VNet \
--address-prefixes 10.0.4.0/24
```

Hub-Management-Subnet:
```bash
az network vnet subnet create \
--name Hub-Management-Subnet \
--resource-group MedCenter \
--vnet-name Hub-VNet \
--address-prefixes 10.0.10.0/24
```

Used `az network vnet show` command to list my created VNet and subnets to confirm all of it was created:
```bash
az network vnet show \
--resource-group MedCenter \
--name Hub-VNet \
--query "subnets"
```

Output:

<img width="947" height="591" alt="image" src="https://github.com/user-attachments/assets/c0caa475-7792-425f-884a-3d169c70c905" />

<img width="947" height="741" alt="image" src="https://github.com/user-attachments/assets/a2c5a86c-f4e1-4636-a59d-8485664675c4" />

2. Created `Pharmacy`, `Research` and `On-prem Mock` VNets with their subnets:

* Pharmacy VNet:
```bash
az network vnet create \
--name Pharmacy-VNet \
--resource-group MedCenter \
--address-prefixes 10.1.0.0/16
```

Added subnet:
```bash
az network vnet subnet create \
--name Pharmacy-Subnet \
--resource-group MedCenter \
--vnet-name Pharmacy-VNet \
--address-prefixes 10.1.0.0/24
```

* Research VNet:
```bash
az network vnet create \
--name Research-VNet \
--resource-group MedCenter \
--address-prefixes 10.2.0.0/16
```

Added subnet:
```bash
az network vnet subnet create \
--name Research-Subnet \
--resource-group MedCenter \
--vnet-name Research-VNet \
--address-prefixes 10.2.0.0/24
```

* On prem Mock VNet, but created it in a different region and RG to simulate on prem:
```
az network vnet create \
--name OnPrem-Mock-VNet \
--resource-group MedCenter-OnPrem \
--location westeurope \
--address-prefixes 10.10.0.0/16
```

Added subnet:
```bash
az network vnet subnet create \
--name OnPrem-Subnet \
--resource-group MedCenter-OnPrem \
--vnet-name OnPrem-Mock-VNet \
--address-prefixes 10.10.0.0/24
```

Confirmed in the portal the existance of the VNets:

<img width="684" height="161" alt="image" src="https://github.com/user-attachments/assets/f606ed5a-81ba-458a-88c6-b655819a1fdb" />

As well as subnets:

* Hub-VNet
<img width="668" height="160" alt="image" src="https://github.com/user-attachments/assets/22fe0e11-a220-4173-ada0-e234c73d67f0" />

* OnPrem-Mock-VNet
<img width="701" height="43" alt="image" src="https://github.com/user-attachments/assets/489d5c34-0aef-4616-9025-d66d6a814009" />

* Pharmacy-VNet
<img width="642" height="71" alt="image" src="https://github.com/user-attachments/assets/5cb6161d-50db-49ed-828a-b704479cb596" />

* Research-VNet
<img width="625" height="79" alt="image" src="https://github.com/user-attachments/assets/1532a4a5-d020-4377-a481-f3874467a2bd" />

## Deploying firewall in the Hub

1. Created Public IP for the firewall
```bash
az network public-ip create \
--resource-group MedCenter \
--name Hub-FW-PIP \
--sku standard \
--allocation-method static
```

Output:

<img width="942" height="646" alt="image" src="https://github.com/user-attachments/assets/584e3a31-c8cf-4021-93e8-aa9844b2fb4d" />

**Note for self**

FW only support standard SKUs

2. Deployed AZ firewall
```bash
az network firewall create \
--name Hub-Firewall \
--resource-group MedCenter \
--location norwayeast \
--sku AZFW_VNet \
--tier Standard \
--vnet-name Hub-VNet
```

Output:

<img width="945" height="653" alt="image" src="https://github.com/user-attachments/assets/7540de90-6279-4e72-8e09-36390e76e66e" />

3. Attached the public IP to the firewall
```bash
az network firewall ip-config create \
--firewall-name Hub-Firewall \
--resource-group MedCenter \
--name FW-IPConfig \
--public-ip-address Hub-FW-PIP \
--vnet-name Hub-VNet
```

Checked if firewall was provisioned
```bash
az network firewall show \
--name Hub-Firewall \
--resource-group MedCenter \
--query "provisioningState"
```

Took around 10 mins:

<img width="168" height="60" alt="image" src="https://github.com/user-attachments/assets/3b741040-b915-43b8-ac37-93502b28abb2" />

4. Applied the IP config to the firewall
```bash
az network firewall update \
--name Hub-Firewall \
--resource-group MedCenter
```

<img width="946" height="768" alt="image" src="https://github.com/user-attachments/assets/91b2c7e5-d9cb-42fc-91ac-8cd114e4003f" />

5. Got the firewalls private IP for later UDRs. All internal routing will be pointed to it and it becomes as the next hop for all traffic i would want to get inspected.
```bash
az network firewall show \
--name Hub-Firewall \
--resource-group MedCenter \
--query "provisioningState"
```

Noted the private IP of the firewall for myself.

## Creating UDRs

I needed to create user defined routes or UDR to force all traffic through the firewall i created.

Currently my spokes would send traffic to each other or directly to the internet which violates compliance requirements.

Basically needed to create route that would say "doesnt matter where you want to go, you go to the firewall first".

1. Created route table for Pharmacy spoke
```bash
az network route-table create \
--resource-group MedCenter \
--name Pharmacy-RT \
--location norwayeast
```

Added routes `Internet > firewall`
```bash
az network route-table route create \
--resource-group MedCenter \
--route-table-name Pharmacy-RT \
--name InternetToFirewall \
--address-prefix 0.0.0.0/0 \
--next-hop-type VirtualAppliance \
--next-hop-ip-address 10.0.1.4
```

Route: `research > firewall`
```bash
az network route-table route create \
--resource-group MedCenter \
--route-table-name Pharmacy-RT \
--name ResearchToFirewall \
--address-prefix 10.2.0.0/16 \
--next-hop-type VirtualAppliance \
--next-hop-ip-address 10.0.1.4
```

Associated with Pharmacy subnet
```bash
az network vnet subnet update \
--resource-group MedCenter \
--vnet-name Pharmacy-VNet \
--name Pharmacy-Subnet \
--route-table Pharmacy-RT
```
<img width="943" height="413" alt="image" src="https://github.com/user-attachments/assets/98a65256-c65d-46fb-9e9c-6613c5a0ff36" />

2. Created route table for Research spoke
```bash
az network route-table create \
--resource-group MedCenter \
--name Research-RT \
--location norwayeast
```

Route: `Internet > Firewall`
```bash
az network route-table route create \
--resource-group MedCenter \
--route-table-name Research-RT \
--name InternetToFirewall \
--address-prefix 0.0.0.0/0 \
--next-hop-type VirtualAppliance \
--next-hop-ip-address 10.0.1.4
```

Route: `Pharmacy > Firewall`
```bash
az network route-table route create \
--resource-group MedCenter \
--route-table-name Research-RT \
--name PharmacyToFirewall \
--address-prefix 10.1.0.0/16 \
--next-hop-type VirtualAppliance \
--next-hop-ip-address 10.0.1.4
```

Associated with Research subnet
```bash
az network vnet subnet update \
--resource-group MedCenter \
--vnet-name Research-VNet \
--name Research-subnet \
--route-table Research-RT
```
<img width="946" height="411" alt="image" src="https://github.com/user-attachments/assets/4b90211f-5bf2-470b-8513-b684ea117cd2" />

Now i had both spokes send all internet traffic to the firewall, the spokes could send traffic to each other > firewall.

Firewall was now mandatory inspection point and compliance req was met.

## Configuring firewall rules

Right now the UDRs would force all traffic through the firewall, however the firewall blocks everything unless explictly allowed. So thats what im doing here.

I needed to add:

* Rule to allow `Pharmacy`<>`Research` communication
* Rule to allow internet outbound so VMs could update, ping, curl etc
* Rule to allow DNS which is required for internet access

1. Created firewall policy
```bash
az network firewall policy create \
--name Hub-FW-Policy \
--resource-group MedCenter \
--location norwayeast
```
<img width="945" height="379" alt="image" src="https://github.com/user-attachments/assets/667aabfc-c1be-4933-8cca-d1914dab8316" />

2. Attached the new policy to the firewall
```bash
az network firewall update \
--name Hub-Firewall \
--resource-group MedCenter \
--firewall-policy $(az network firewall policy show \
--name Hub-FW-Policy \
--resource-group RG-Hub \
--query id -o tsv)
```
<img width="946" height="807" alt="image" src="https://github.com/user-attachments/assets/f933e2d5-3253-47b9-9f41-4e8dd9715983" />

**Note to self**

Policy cant be attached without full resource ID. The above command fetches the full resource ID and applies it to the firewall using combination of outer and inner command.

3. Created network rule collection which would allow VM-to-VM traffic aka `pharmacy`<>`research`
```bash
az network firewall policy rule-collection-group create \
--policy-name Hub-FW-Policy \
--resource-group MedCenter \
--name Hub-NetworkRules \
--priority 100
```
<img width="945" height="322" alt="image" src="https://github.com/user-attachments/assets/c5fdbaec-df15-4db5-aff7-721b77001403" />

This took around 5 mins

4. Added rules to allow inter-spoke traffic

Created filter rule collection in the group. Cant add rules before defining the collection and the collections need to be created with at least 1 rule already inside them before you can add more.

Pharmacy > Research
```bash
az network firewall policy rule-collection-group collection add-filter-collection \
--policy-name Hub-FW-Policy \
--resource-group MedCenter \
--rule-collection-group-name Hub-NetworkRules \
--name Hub-Network-Collection \
--collection-priority 100 \
--action Allow \
--rule-name PharmacyToResearch \
--rule-type NetworkRule \
--source-addresses 10.1.0.0/16 \
--destination-addresses 10.2.0.0/16 \
--destination-ports '*' \
--ip-protocols Any
```
<img width="944" height="835" alt="image" src="https://github.com/user-attachments/assets/99182c89-75e4-4d7d-96ea-690f5c44d3b1" />

Research > Pharmacy
```bash
az network firewall policy rule-collection-group collection rule add \
  --policy-name Hub-FW-Policy \
  --resource-group MedCenter \
  --rule-collection-group-name Hub-NetworkRules \
  --collection-name Hub-Network-Collection \
  --name ResearchToPharmacy \
  --rule-type NetworkRule \
  --source-addresses 10.2.0.0/16 \
  --destination-addresses 10.1.0.0/16 \
  --destination-ports '*' \
  --ip-protocols Any
```
<img width="945" height="838" alt="image" src="https://github.com/user-attachments/assets/b71c1278-31f7-4a99-a70f-95860c8d74e2" />

<img width="945" height="424" alt="image" src="https://github.com/user-attachments/assets/ac6d48be-2d39-4b87-a22e-7781dfb4c2c7" />

6. Needed to allow DNS for the internet
```bash
az network firewall policy rule-collection-group collection rule add \
--policy-name Hub-FW-Policy \
--resource-group MedCenter \
--rule-collection-group-name Hub-NetworkRules \
--collection-name Hub-Network-Collection \
--name Allow-DNS \
--rule-type NetworkRule \
--source-addresses '*' \
--destination-addresses 168.63.129.16 \
--destination-ports 53 \
--ip-protocols UDP
```
<img width="944" height="840" alt="image" src="https://github.com/user-attachments/assets/3ee3ab60-920c-4c01-8544-b1b9c09e9f3d" />

<img width="944" height="756" alt="image" src="https://github.com/user-attachments/assets/6b4dab2c-0521-40d4-8a7c-41e08147bd66" />

**Note to self**

168.63.129.16 is Azures platform services endpoint. Every VM, firewall, and resource in AZ relies on it for core functionality and that includes DNS.

* `--priority` is not a valid parameter for individual rules.
* `--rule-name` is not supported
* `--action` is not supported at rule level

7. Allowed outbound internet so VMs could reach the it through the firewall
```bash
az network firewall policy rule-collection-group collection rule add \
  --policy-name Hub-FW-Policy \
  --resource-group MedCenter \
  --rule-collection-group-name Hub-NetworkRules \
  --collection-name Hub-Network-Collection \
  --name Allow-Internet \
  --rule-type NetworkRule \
  --source-addresses 10.0.0.0/8 \
  --destination-addresses '*' \
  --destination-ports '*' \
  --ip-protocols Any
```
<img width="945" height="802" alt="image" src="https://github.com/user-attachments/assets/56cf1283-905b-4077-b45b-bda6b43c7601" />

<img width="944" height="805" alt="image" src="https://github.com/user-attachments/assets/23d7835b-eea2-40a0-a919-e2d9d14935ad" />

<img width="944" height="317" alt="image" src="https://github.com/user-attachments/assets/615b1af0-fa78-4517-9264-1dbc66d2cadc" />

Used 10.0.0.0/8 to apply rule to all addresses in that range instead of doing manual labor.

My firewall now allowed `Pharmacy` <> `Research` communication, DNS, outbound internet. Would log all traffic and enforce compliance.

With the UDRs in combination with firewall rules created a fully inspected hub and spoke network.

## Deploying VPN gateway + S2S connection.

Now i wanted to connect the Mock on prem VNet to the Hub VNet to complete hybrid set up.

So i needed to create vpn gateway both in the Hub and in the On prem mockup. Establish local network gateway on each side, create connection between them and create a PSK.

1. Created gateway public IP
```bash
az network public-ip create \
--resource-group MedCenter \
--name Hub-GW-PIP \
--allocation-method Static
```
<img width="940" height="630" alt="image" src="https://github.com/user-attachments/assets/fb9d7e94-d0de-4918-aeab-c133b0dc406b" />

2. Created mock gateway public IP in OnPrems RG
```bash
az network public-ip create \
--resource-group MedCenter-OnPrem \
--name OnPrem-GW-PIP \
--allocation-method Static
```
<img width="944" height="599" alt="image" src="https://github.com/user-attachments/assets/b2818d5e-8ad3-411d-bb92-227817c5abb7" />

3. Deleted main subnet `10.0.0.0/16` from the On prem mock
```bash
az network vnet subnet delete \
--resource-group MedCenter-OnPrem \
--vnet-name OnPrem-Mock-VNet \
--name OnPrem-Subnet
```
Needed to delete it because GW couldnt be created since the address space used up all the space (pun not intended)

Recreated it in smaller CIDR
```bash
az network vnet subnet create \
--resource-group MedCenter-OnPrem \
--vnet-name OnPrem-Mock-VNet \
--name OnPrem-Subnet \
--address-prefixes 10.10.0.0/24
```
<img width="942" height="343" alt="image" src="https://github.com/user-attachments/assets/8cd0094f-f063-4eb7-ab57-2305e47469ac" />

Then created GateWaySubnet for on prem mock VNet
```bash
az network vnet subnet create \
--resource-group MedCenter-OnPrem \
--vnet-name OnPrem-Mock-VNet \
--name GatewaySubnet \
--address-prefixes 10.10.255.0/27
```
<img width="945" height="348" alt="image" src="https://github.com/user-attachments/assets/1d6cb264-9a48-4e3b-8fcf-c75e7351e0fd" />

**Note to self**

All VPN gateways require their dedicated subnets.

4. Created VPN gateways

Hub VPN gateway
```bash
az network vnet-gateway create \
--resource-group MedCenter \
--name Hub-VPN-Gateway \
--vnet Hub-VNet \
--public-ip-address Hub-GW-PIP \
--gateway-type Vpn \
--vpn-type RouteBased \
--sku VpnGw1 \
--no-wait
```

OnPrem-Mock VPN gateway
```bash
az network vnet-gateway create \
--resource-group MedCenter-OnPrem \
--name OnPrem-VPN-Gateway \
--vnet OnPrem-Mock-VNet \
--public-ip-address OnPrem-GW-PIP \
--gateway-type Vpn \
--vpn-type RouteBased \
--sku VpnGw1 \
--no-wait
```

**Note to self** 

For public IP address and VPN gateway creations, both need to exist in the same region to work. You cant create VPN gateway in 1 region if the public IP resides in a different region.

Verified that both VPN gateways now existed with all the configurations

Hub VPN gateway
```bash
az network vnet-gateway show \
--resource-group MedCenter \
--name Hub-VPN-Gateway \
--output table
```
<img width="1903" height="200" alt="image" src="https://github.com/user-attachments/assets/47ee03e4-59fd-42ff-91c8-5c53e140c68a" />

Gateway was provisioned in norway east

OnPrem VPN gateway
```bash
az network vnet-gateway show \
--resource-group MedCenter-OnPrem \
--name OnPrem-VPN-Gateway \
--output table
```
<img width="1906" height="199" alt="image" src="https://github.com/user-attachments/assets/e12a1afb-69c5-414c-b430-1ead5e55e7fd" />

On prem was updating but the configurations were correct and location. VPN gateways usually take around 20 mins to fully provision.

## Created local network gateways

These would represent the other side of the tunnel so to speak and would tell both gateways respectively where to send encrypted traffic. For this to work you need to know VPN gateways public IPs for both which i had copied earlier when i created public IPs, into my notepad so i wouldnt have to go through the hassle of running commands seperately to find it out.

On the Hub side (representing OnPrem)
```bash
az network local-gateway create \
  --resource-group MedCenter \
  --name OnPrem-LNG \
  --gateway-ip-address 20.126.101.41 \
  --local-address-prefixes 10.10.0.0/16 \
  --location norwayeast
```
<img width="943" height="434" alt="image" src="https://github.com/user-attachments/assets/9f011db3-e27c-4f24-a9c9-57831ec7c09e" />

**Note to self**

LNGs region is very important because it determines where the LNG resource itself lives. Local Network Gateway must be created in the same region as the Virtual Network Gateway that will use it. It doesnt matter where the remote network lives or what address prefixes it has, those are just configuration details.

On the OnPrem side (representing Hub)
```bash
az network local-gateway create \
  --resource-group MedCenter-OnPrem \
  --name Hub-LNG \
  --gateway-ip-address 20.100.174.241 \
  --local-address-prefixes 10.0.0.0/16 \
  --location westeurope
```
<img width="944" height="432" alt="image" src="https://github.com/user-attachments/assets/c1186ba3-466a-4c56-8ea1-0937651007b9" />

**Note to self**

Each local network gateway describes the other side of the tunnel, basically mirroring it.

## Created VPN connections

**Note to self**

Both sides of the tunnel need to use same shared key

Checked my public IP list and local gateway lists to make sure everything was perfect before creating vpn connections

**Public IP list**

**MedCenter**

<img width="885" height="148" alt="image" src="https://github.com/user-attachments/assets/f30d93c4-6131-427a-922f-80106e2927f9" />

**LNG list MedCenter**

<img width="803" height="132" alt="image" src="https://github.com/user-attachments/assets/5ed728be-2002-4e25-8590-36225693c4a9" />

**MedCenter-OnPrem**

<img width="839" height="127" alt="image" src="https://github.com/user-attachments/assets/8a6bc99d-e992-4bbd-bf5b-609a28f0a5f2" />

**LNG list of MedCenter-OnPrem**

<img width="791" height="127" alt="image" src="https://github.com/user-attachments/assets/d154b544-7280-4eee-9849-ec7099b9d9f7" />

Hub > OnPrem
```bash
az network vpn-connection create \
--resource-group MedCenter \
--name Hub-To-OnPrem \
--vnet-gateway1 Hub-VPN-Gateway \
--local-gateway2 OnPrem-LNG \
--shared-key "MyPWwhichIwillNotDiscloseHere"
```
<img width="945" height="636" alt="image" src="https://github.com/user-attachments/assets/5ca717af-8ea5-413a-9dfe-2eace62c931c" />

OnPrem > Hub
```bash
az network vpn-connection create \
--resource-group MedCenter-OnPrem \
--name OnPrem-To-Hub \
--vnet-gateway1 OnPrem-VPN-Gateway \
--local-gateway2 Hub-LNG \
--shared-key "Kadgu7hbdzkyn1ax"
```
<img width="943" height="633" alt="image" src="https://github.com/user-attachments/assets/815d419a-c124-4e1b-b486-b5ea1d49d315" />

Verified that both were now connected

**Hub-To-OnPrem**

<img width="667" height="124" alt="image" src="https://github.com/user-attachments/assets/7e3bbd85-f0ca-448c-9e1a-61dbb118bf70" />

**OnPrem-To-Hub**

<img width="615" height="52" alt="image" src="https://github.com/user-attachments/assets/c9fe1504-06e4-479f-bbfb-14887b4efde5" />

Created VMs for both Hub and OnPrem to test that tunnel worked 100%

**Hub VM**
```bash
az vm create \
--resource-group MedCenter \
--name HubTestVM \
--image Ubuntu2204 \
--vnet-name Hub-VNet \
--subnet Hub-Subnet \
--size Standard_B1s \
--admin-username azureuser \
--generate-ssh-keys \
--public-ip-address ""
```

**OnPrem VM**
```bash
az vm create \
--resource-group MedCenter-OnPrem \
--name OnPremTestVM \
--image Ubuntu2204 \
--vnet-name OnPrem-Mock-VNet \
--subnet OnPrem-Subnet \
--size Standard_B1s \
--admin-username azureuser \
--generate-ssh-keys \
--public-ip-address ""
```

Next i got the private IPs for both VMs

**Hub VM private IP**
```bash
az vm list-ip-addresses \
--resource-group MedCenter \
--name HubTestVM \
--query "[].virtualMachine.network.privateIpAddresses[]" \
--output tsv
```
<img width="476" height="114" alt="image" src="https://github.com/user-attachments/assets/ca99138e-fbd9-4254-a085-616c11e1c8dc" />

**OnPrem VM private IP**
```bash
az vm list-ip-addresses \
--resource-group MedCenter-OnPrem \
--name OnPremTestVM \
--query "[].virtualMachine.network.privateIpAddresses[]" \
--output tsv
```
<img width="476" height="108" alt="image" src="https://github.com/user-attachments/assets/5ed3173d-e405-46c2-abf6-d8ab2db4a015" />

Went to portal in the Hubs VM and under `operations` > `run command`, then picked `runshellscript`.

ran the following to test connection
```bash
ping -c 4 10.10.0.4
```
<img width="949" height="726" alt="image" src="https://github.com/user-attachments/assets/f7300625-af4c-4496-b7a7-550fa2bf3e7d" />

Checked routes
```bash
ip route
```
<img width="757" height="613" alt="image" src="https://github.com/user-attachments/assets/3fc6ad2c-1be7-4685-bcc1-33e675ddb05f" />

Did the same for OnPrem
```bash
ping -c 4 10.0.0.4
```
<img width="955" height="704" alt="image" src="https://github.com/user-attachments/assets/7734cedc-89a8-49dd-bbfa-d3f9a5656308" />

Checked routes
```bash
ip route
```
<img width="756" height="514" alt="image" src="https://github.com/user-attachments/assets/80d44b8b-b1d1-4fdc-a7aa-b8cd821b713c" />

I wanted to use bastion here but my internet couldnt let me and connection timed out, luckily being able to run commands in portal without having to go inside the VM itself is pretty awesome ngl because it saves time.

Now i knew that tunnel was working as intended, connection was established, gateways were working and i could move on to deploing appliocation gateway with WAF.

## Deploying application gateway + WAF

1. Created public IP for the app gateway
```
az network public-ip create\
--resource-group MedCenter \
--name AppGW-PIP \
--sku Standard \
--allocation-method Static
```

2. Created WAF policy
```bash
az network application-gateway waf-policy create \
--name Pharmacy-WAF-Policy \
--resource-group MedCenter \
--location norwayeast
```
<img width="943" height="653" alt="image" src="https://github.com/user-attachments/assets/0d604e5f-b452-43c9-b72c-888cbc9b8673" />

3. Created the app gateway with waf enabled and attached the policy
```bash
az network application-gateway create \
--name Pharmacy-AppGW \
--location norwayeast \
--resource-group MedCenter \
--vnet-name Hub-VNet \
--subnet AppGatewaySubnet \
--capacity 1 \
--sku WAF_v2 \
--public-ip-address AppGW-PIP \
--priority 100 \
--waf-policy Pharmacy-WAF-Policy
```

**Note to self**

Terminal will disconnect when deploying gateway with WAF.

4. Created Pharmacy-VM that i would later add to the backend pool
```bash
az vm create \
--resource-group MedCenter \
--name Pharmacy-VM \
--image Ubuntu2204 \
--vnet-name Hub-VNet \
--subnet Hub-Subnet \
--size Standard_B1s \
--admin-username azureuser \
--generate-ssh-keys \
--public-ip-address ""
```

5. Installed web server on the Pharmacy VM by running commands in the operations
```bash
sudo apt update
```
```bash
sudo apt install nginx -y
```
```bash
sudo systemctl enable nginx
```
<img width="740" height="463" alt="image" src="https://github.com/user-attachments/assets/1aedb4d9-a88b-40a6-bf0c-f1e1ed1e7555" />

```bash
sudo systemctl start nginx
```
<img width="740" height="429" alt="image" src="https://github.com/user-attachments/assets/f2d740c2-9008-4bea-a327-92a055e041b0" />

6. Added Pharmacy‑VM to the backend pool, i had saved private IP prior
```bash
az network application-gateway address-pool update \
--gateway-name Pharmacy-AppGW \
--resource-group MedCenter \
--name appGatewayBackendPool \
--servers 10.0.0.5
```
<img width="946" height="366" alt="image" src="https://github.com/user-attachments/assets/213b9e01-4878-4fe5-abc0-887a487fc769" />

7. Updated HTTP settings to ensure it would match the backend
```bash
az network application-gateway http-settings update \
--gateway-name Pharmacy-AppGW \
--resource-group MedCenter \
--name appGatewayBackendHttpSettings \
--port 80 \
--protocol Http \
--timeout 30
```
<img width="944" height="530" alt="image" src="https://github.com/user-attachments/assets/7ac0d772-1136-47f2-bfcf-86c836ce49e1" />

8. Checked the health of backend by going in the portal `Pharmacy-AppGW` > `Backend health` under `Monitoring`:

<img width="1364" height="424" alt="image" src="https://github.com/user-attachments/assets/2096d558-8447-4ab2-a0b9-9f489e03a3ba" />

9. Next i tested the App gateway
```bash
az network public-ip show \
--resource-group MedCenter \
--name AppGW-PIP \
--query "ipAddress" \
--output tsv
```

Copied the IP to another browser aaand:

<img width="786" height="348" alt="image" src="https://github.com/user-attachments/assets/1e6d4c7f-1994-4edd-a720-0b1ea46220d6" />


In short i created public IP for the app gateway, enabled web app firewall in the Hub. Pharmacy VM was added as backend with listener and routing rule and it could only be reached through the app gateway meaning that compliance was met.

## Application security groups and NSG rules

ASGs would let me group the VMs by function and in return the NSGs could target ASGs instead of individual IPs which is much more cleaner and scalable way of doing things.

1. Created ASG for the pharmacy web
```bash
az network asg create \
--resource-group MedCenter \
--name asg-pharmacy-web \
--location norwayeast
```
<img width="944" height="206" alt="image" src="https://github.com/user-attachments/assets/2c5800ad-bc12-47d9-9580-7e007e1ab102" />

2. Added pharmacys VM NIC to the ASG

First i got the nic
```bash
az vm show \
--resource-group MedCenter \
--name Pharmacy-VM \
--query "networkProfile.networkInterfaces[0].id" \
--output tsv
```

Then got the ASG resource ID which i copied
```bash
az network asg show \
--resource-group MedCenter \
--name asg-pharmacy-web \
--query "id" \
--output tsv
```

Added the pharmacy VM NIC to ASG
```bash
az network nic ip-config update \
--nic-name Pharmacy-VMVMNic \
--resource-group MedCenter \
--name ipconfigPharmacy-VM \
--application-security-groups "/subscriptions/f37270cc-38a5-41a1-bd72-30ffc482d6c2/resourceGroups/MedCenter/providers/Microsoft.Network/applicationSecurityGroups/asg-pharmacy-web"
```
<img width="946" height="582" alt="image" src="https://github.com/user-attachments/assets/36c127cb-79c7-49b4-adaa-29dfd7846b90" />

It turns out that i had to use this command because azure stores ASG membership inside the NICs IP configuration, and my CLI only supports updating ASGs at that level and not on the NIC root object.

3. Identified NSG on the pharmacy subnet

I had to apply NSG rules at the subnet level so the Application Gateway could reach the VM.
Azure evaluates NSG rules on both the NIC and the subnet, and the subnet NSG is the one that controls inbound traffic from the App Gateway.

Got the NSG id
```bash
az network vnet subnet show \
--resource-group MedCenter \
--vnet-name Hub-VNet \
--name Hub-Subnet \
--query "networkSecurityGroup.id" \
--output tsv
```

However it returned nothing, which meant that no NSG was associated with that subnet, so i needed to create NSG for it.

```bash
az network nsg create \
--resource-group MedCenter \
--name Hub-Subnet-NSG \
--location norwayeast
```

Then associated the NSG with the subnet
```bash
az network vnet subnet update \
--resource-group MedCenter \
--vnet-name Hub-VNet \
--name Hub-Subnet \
--network-security-group Hub-Subnet-NSG
```
<img width="946" height="651" alt="image" src="https://github.com/user-attachments/assets/31c16bfc-490f-4809-a1cd-55665aff29b7" />

4. Added Allow HTTP 80 and HTTPS 443 NSG rules that would target the ASG

**Allow HTTP 80**
```bash
az network nsg rule create \
--resource-group MedCenter \
--nsg-name Hub-Subnet-NSG \
--name Allow-AppGW-HTTP \
--priority 200 \
--direction Inbound \
--access Allow \
--protocol Tcp \
--source-address-prefixes 10.0.4.0/24 \
--destination-asgs asg-pharmacy-web \
--destination-port-ranges 80
```
<img width="946" height="709" alt="image" src="https://github.com/user-attachments/assets/3a80504e-9b7a-4780-a62c-5a9e130de21c" />

**Allow HTTPS 443**
```bash
az network nsg rule create \
--resource-group MedCenter \
--nsg-name Hub-Subnet-NSG \
--name Allow-AppGW-HTTPS \
--priority 210 \
--direction Inbound \
--access Allow \
--protocol Tcp \
--source-address-prefixes 10.0.4.0/24 \
--destination-asgs asg-pharmacy-web \
--destination-port-ranges 443
```
<img width="945" height="714" alt="image" src="https://github.com/user-attachments/assets/669a7041-b352-48d1-b22a-aaa0b518836a" />

Basically, i used ASGs to manager traffic rules for the web servers in the Pharmacy spoke. 

## Deploying AZ Bastion for secure management of VMs

By deploying Bastion i could RDP/SSH into any VM without public IPs.

**Note to self**

Bastions live in the hub because the hubs are shared management planes.

1. Created public IP for Bastion
```bash
az network public-ip create \
--resource-group MedCenter \
--name Bastion-PIP \
--sku Standard \
--allocation-method Static
```
<img width="941" height="625" alt="image" src="https://github.com/user-attachments/assets/f3f7e3ed-161f-4be8-bc57-87e8c9ad1b7d" />

2. Deployed bastion in the Hub
```bash
az network bastion create \
--name Hub-Bastion \
--resource-group MedCenter \
--location norwayeast \
--vnet-name Hub-VNet \
--subnet AzureBastionSubnet \
--public-ip-address Bastion-PIP \
--sku Basic
```

But it returned with:

<img width="592" height="183" alt="image" src="https://github.com/user-attachments/assets/aad6918f-75d5-4904-95e0-b2325f7485bd" />

So i got confused because i didnt remember deploying bastion before hand. I verified i indeed had bastion
```bash
az network bastion list \
--resource-group MedCenter \
--output table
```
<img width="913" height="126" alt="image" src="https://github.com/user-attachments/assets/c58d6c70-9103-4984-866c-f76b5c646cbf" />

So i tried to trace back and think on how this might have happened, and i remembered that earlier i when i wanted to `ip route` inside the test VMs i clicked on `Deploy bastion` in the portal and it turns out that when you do that then bastion is deployed automatically with its subnet. 

Pretty neat huh?

Anyway the project itself was done, i had verified every deployment, made sure everything worked as intended, documented it all and now my next goal was to automate the entire thing.

For now i performed cleanup by nuking both resource groups and i could now take a break.

### Troubleshooting

I kept running into error:
`Message: /subscriptions/f37270cc-38a5-41a1-bd7230ffc482d6c2/resourceGroups/MEDCENTER/providers/Microsoft.Network/virtualNetworks/ONPREM-MOCK-VNET
`

I double checked every configuration, from public IPs, LNGs, regions to gateway names and everything was correct. The issue wasnt the setup, it was ARM holding onto stale, corrupted dependency data from earlier failed VPN deployments. 

Cleaning the deployments didnt help, so the only reliable fix was to delete the entire On Prem side and rebuild it in a separate resource group. Splitting Hub and On Prem into different RGs avoids ARMs cached dependency issues and makes the VPN connections deploy cleanly.

I troubleshooted this for hours and felt like i was going crazy and the takeaway here is, if you simulate on prem... please do it in a seperate resource group because otherwise youll have to rebuild everything. Luckily i know im getting a clean resource group to rebuild on prem mock with no hassle.

Instead of walking through setting up the On prem side in a different RG i just made changes to the steps before.

PS: AZ firewall is really goddamn expensive, wow!
