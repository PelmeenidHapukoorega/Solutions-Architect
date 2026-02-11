# Notes about configuring DNS to host domain step by step

## Step 1: Creating DNS zone in AZ

To host domain, DNS zone needs to be created. DNS zone holds all the DNS entries for the domain.

When creating DNS zone following details need to be supplied:

* Subscription that will be used
* Resource group that will hold the domains.
* Name for the domain
* Resource group location

## Step 2: Getting Azure DNS name servers

After DNS zone creation for the domain, you need to get name server details from the NS record.
These details are then used to update domain registrars information and the pointed to the AZ DNS zone.

## Step 3: Updating domain registrars settings

Domain owner needs to sign in to the domain-management application provided by the domain registrar.

In the management application, edit NS record and change the NS details to match DNS name server details.

Changing NS details is called `domain delegation`. When delegating domain, all 4 name servers must be used which are provided by the DNS

## Step 4: Verifying delegation of DNS

Process can take around 10 mins, but maybe longer.

To verify success of domain delegation, query the SOA record. SOA record is automatically created when the DNS zone is set up.
You can verify SOA record using `nslookup` in the terminal.

SOA record represents your domain and becomes reference point when other DNS servers are reaching for you domain on the internet.

To verify delegation use:
```PWSH
nslookup -type=SOA wideworldimports.com <(Your domain name)
```

## Step 5: Configuring custom DNS settings

The domain name `wwww.(yourdomainname).com` when used in browser it will resolve to your website.

What if you wanted to add in web servers or load balancers? These need to have their own custom settings in the DNS zone, either as `A` or `CNAME` record.

**A record**

Each A record requires following details:

* **Name**
  * Name of the custom domain, like `webserver1`
* **Type**
  * In this instance its A
* **TTL**
  * Represents "time-to-live" as a whole unit, where 1 is a 1 second.
  * Value indicates how long the A record lives in the DNS cache before expiration.
* **IP address**
  * IP address of the server to which this A record should resolve

**CNAME record**

Used when having different domain names that all access same website.

You create CNAME record with following info:

* Name: www
* TTL: 600 seconds
* Record type: CNAME

If exposed a web function, create CNAME record that resolves to AZ function.

## Configuring Private DNS zone

Private DNS zones arent visible on the internet and dont require domain registrar. Can be used to assign DNS names to VMs in the VNet.

## Step 1: Creating private DNS zone

Search for `private DNS zones` in the portal. Needs resource group and name of the zone.

## Step 2: Identifying VNets

Identify VNet associated with VMs that need name-resolution support.

To link VNets to the private zone, you need VNet names.

## Step 3: Linking VNet to private DNS zone

To link private DNS zone to VNet you need to create VNet link. Under `Private zone` > `virtual network link` then > `add` and pick the VNet you want to link with privated zone.

You add VNet link record to each VNet that needs private name-resolution support.
