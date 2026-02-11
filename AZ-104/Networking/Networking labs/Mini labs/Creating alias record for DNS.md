# Mini lab: Creating alias record for AZ DNS

1. Opened up CLI in the portal and ran the set up script:
```bash
git clone https://github.com/MicrosoftDocs/mslearn-host-domain-azure-dns.git
```

Output:

<img width="832" height="159" alt="image" src="https://github.com/user-attachments/assets/d4fcfd26-e592-4e17-ab22-4b12f4abcc18" />

2. Then ran the script:
```bash
cd mslearn-host-domain-azure-dns
chmod +x setup.sh
./setup.sh
```

Output:


This created:

* NSG
* 2 NICs and 2 VMs
* VNet and assigns the VMs to it.
* Public IP address and updated config to VMs.
* Load balancer that would reference VMs, including rules for the load balancer.
* Links the NICs to the load balancer.

3. Once the set up was complete i copied the IP address  `20.100.195.91` to use later.
4. Went to my  `NetworkWatcherRG` and selected 
