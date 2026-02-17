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

2. Created `Pharmacy`, `Research` and `On-prem Mock` with their subnets:

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

* On prem Mock VNet, but created it in a different region to simulate on prem:
```
az network vnet create \
--name OnPrem-Mock-VNet \
--resource-group MedCenter \
--location westeurope \
--address-prefixes 10.10.0.0/16
```

Added subnet:
```bash
az network vnet subnet create \
--name OnPrem-Subnet \
--resource-group MedCenter \
--vnet-name OnPrem-Mock-VNet \
--address-prefixes 10.10.0.0/24
```

Confirmed in the portal the existance of the VNets:

<img width="656" height="191" alt="image" src="https://github.com/user-attachments/assets/d502a596-515b-436f-8fd7-db3adfb8d963" />

As well as subnets:

* Hub-VNet
<img width="644" height="192" alt="image" src="https://github.com/user-attachments/assets/b88a5a78-02b1-4913-9923-7832400f7845" />

* OnPrem-Mock-VNet
<img width="630" height="78" alt="image" src="https://github.com/user-attachments/assets/fedc279d-dc56-4328-9a6e-ffa6cb1676ce" />

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

Note the private IP of the firewall for myself.

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

My firewall now allowed Pharmacy <> Research communication, DNS, outbound internet. Would log all traffic and enforce compliance.

With the UDRs in combination with firewall rules created a fully inspected hub and spoke network.

## Deploying VPN gateway + S2S connection.

Now i wanted to connect the Mock on prem VNet to the Hub to complete hybrid set up.

