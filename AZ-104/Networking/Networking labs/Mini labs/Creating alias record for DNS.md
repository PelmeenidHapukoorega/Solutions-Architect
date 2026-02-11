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
4. Went to my  `NetworkWatcherRG` and selected my `Mmerch.com` dns zone i created earlier, then added new record to it by taking defaults, enabling Alias record and leaving name blank:

<img width="544" height="598" alt="image" src="https://github.com/user-attachments/assets/dc01bd65-534a-4a7a-990a-f57a20b2f4ea" />

Hit add

<img width="996" height="79" alt="image" src="https://github.com/user-attachments/assets/2ce4bb4f-351c-4ffc-892a-4e622b394588" />

5. Verified that alias resolved to load balancer by going to the resource grouo, selecting `myPublicIP` then copying its IP address and pasting it to browser which returned me the basic webpage with VM name as `VMtest1`

### Summary

### What i learned

