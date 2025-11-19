# Module: Azure compute and networking services
# Configuring network access for VM and accessing web server lab notes

To actually understand networking in Azure you gotta break a VM on purpose and then fix it. NSGs, ports, SSH, inbound rules this is where the cloud learning begins. 

**Bottom line:** If you can troubleshoot why a VM wont respond, you’re already thinking like someone who builds and maintains real systems.

**Objective:** Show that I can deploy a VM, set up Nginx, and configure the network around it so I fully understand how traffic actually reaches a service running inside Azure.


### Task 1  Retrieved VM IP address
Used `az vm list-ip-addresses` and stored result in Bash variable.

```bash
IPADDRESS="$(az vm list-ip-addresses \
--resource-group "MinuVirtukas" \
--name my-vm \
--query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
--output tsv)"
```

Ran into an error because I misspelled `--resource-group` as `--resource-grp`.
Fixed command worked.
[![Resource Group](../screenshots/error.PNG)](../screenshots/error.PNG)

### Step 2 Tested connection to VM
```bash
curl --connect-timeout 5 http://$IPADDRESS
```
Result:
```
curl: (28) Connection timed out after 5001 milliseconds
```
Reason: NSG didn’t allow inbound port 80 yet.

### Step 3 Optional browser test
```bash
echo $IPADDRESS
```
Copy pasted IP into browser, same unreachable result.
[![Resource Group](../screenshots/unreachable.PNG)](../screenshots/unreachable.PNG)

Two ways to test:
* CURL
* Browser

Both failed correctly because port 80 was closed.

## Task 2 Listing Current Network Security Group (NSG) Rules

### Step 1 Identified NSG name
```bash
az network nsg list \
--resource-group "MinuVirtukas" \
--query '[].name' \
--output tsv
```

Output:
`minu-virtukasNSG`

### Step 2 Listed rules in that NSG
Full JSON version:
```bash
az network nsg rule list \
--resource-group MinuVirtukas \
--nsg-name minu-virtukasNSG
```
 Clean table view:
 ```bash
 az network nsg rule list \
--resource-group MinuVirtukas \
--nsg-name minu-virtukasNSG \
--query "[].{Name:name, Priority:priority, Port:destinationPortRange, Access:access}" \
--output table
```

Output:
```pgsql
Name               Priority    Port    Access
-----------------  ----------  ------  --------
default-allow-ssh  1000        22      Allow
```
**Important:** Default Linux VM NSG only allows SSH (22). 

## Task 3 Created NSG rule for HTTP (Port 80)

Created inbound rule for HTTP:
```bash
az network nsg rule create \
--resource-group MinuVirtukas \
--nsg-name minu-virtukasNSG \
--name allow-http \
--protocol tcp \
--priority 100 \
--destination-port-range 80 \
--access Allow
```

Result:
[![Resource Group](../screenshots/nsgrule.PNG)](../screenshots/nsgrule.PNG)

### Verified rules again:
```bash
Name               Priority    Port    Access
-----------------  ----------  ------  --------
default-allow-ssh  1000        22      Allow
allow-http         100         80      Allow
```

### NSG flow explained:
```pgsql
        Client Request
              |
              v
      +----------------+
      | Public IP NIC  |
      +----------------+
              |
              v
      +----------------+
      | Network NSG    |
      | (inbound rules)|
      +----------------+
              |
   allow? ----+---- deny?
      |                |
      v                v
   Traffic         Blocked
   allowed         (dropped)
```

## Task 4 Accessed Web Server Again

### Step 1 CURL test
```bash
curl --connect-timeout 5 http://$IPADDRESS
```
Now successful:
[![Resource Group](../screenshots/welcome.PNG)](../screenshots/welcome.PNG)

### Step 2 Browser refresh:
[![Resource Group](../screenshots/welcome2.PNG)](../screenshots/welcome2.PNG)

## Challenge Customize Nginx Homepage

Wanted to change the boring default Nginx page.

### Step 1 Retrieved VM IP again
```bash
az vm list-ip-addresses \
--resource-group MinuVirtukas \
--name minu-virtukas \
--query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
--output tsv
```

Output example: `20.251.205.43`

### Step 2 Opened SSH session
```bash
ssh virtualhermit@20.251.205.43
```

First received authenticity warning (which I later found out is normal).
Then got: 
```java 
Permission denied (publickey)
```

**Meaning:** VM accepted connection but my local machine had no matching SSH key.

### Step 3 Retrieved VMs admin username (I forgot it)
```bash
az vm show \
--resource-group MinuVirtukas \
--name minu-virtukas \
--query "osProfile.adminUsername" \
--output tsv
```

Output:
```nginx
virtualhermit
```

### Step 4 Checked which SSH keys Azure expects
```bash
az vm show \
--resource-group MinuVirtukas \
--name minu-virtukas \
--query "osProfile.linuxConfiguration.ssh" \
--output tsv
```

Result:
[![Resource Group](../screenshots/sshconfigret.PNG)](../screenshots/sshconfigret.PNG)

### Step 5 Checked local SSH keys
```bash
ls ~/.ssh
```
Only `known_hosts` existed, no SSH keypair created.

### Step 6 Generated new SSH key
```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -C "azure-vm-key"
```
Left passphrase empty (lab VM):
[![Resource Group](../screenshots/passp.PNG)](../screenshots/passp.PNG)

### Step 7 Added new SSH key to VM user
```bash
az vm user update \
--resource-group MinuVirtukas \
--name minu-virtukas \
--username virtualhermit \
--ssh-key-value "$(cat ~/.ssh/id_ed25519.pub)"
```

### Step 8 SSH into VM again
```bash
ssh virtualhermit@20.251.205.43
```
Now successful login.

### Editing Nginx homepage:
```bash
sudo nano /var/www/html/index.html
```

Final result:
[![Resource Group](../screenshots/final.PNG)](../screenshots/final.PNG)

## What I learned
### Key points
* Public IP retrieval is essential for remote access
* CURL is the fastest way to check web server availability
* NSGs control inbound/outbound flows, nothing works until you open the right ports
* Priority numbers matter: lower number = higher priority
* SSH requires the correct keypair, Azure does NOT accept random new keys unless added
* Knowing how to inspect VM config helps debug authentication issues fast
* Editing Nginx through SSH gives full control of the hosted content

### Takeaway
This lab makes one thing clear: Azure wont magically open ports or fix access issues for you.
If the VM cant be reached, its almost always networking or SSH keys.
Learning to follow the path from IP to NSG to port to service is simply part of getting comfortable with how Azure networking actually works.


## References
### Azure
https://learn.microsoft.com/et-ee/azure/virtual-machines/linux/ssh-from-windows

https://learn.microsoft.com/et-ee/azure/virtual-machines/linux/create-ssh-keys-detailed

https://learn.microsoft.com/en-us/troubleshoot/azure/virtual-machines/linux/troubleshoot-ssh-connection

https://learn.microsoft.com/et-ee/cli/azure/vm?view=azure-cli-latest

### GitHub / Markdown
https://www.markdownguide.org/basic-syntax/

https://docs.github.com/en/get-started/writing-on-github
