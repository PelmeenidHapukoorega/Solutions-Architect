# Mini lab: Creating DNS zone and A record

1. Created new `DNS zone` in `DNS zones` and added it resource group `DNS`
2. Named the zone to `Mmerch.com` then reviewed settings:

<img width="433" height="398" alt="image" src="https://github.com/user-attachments/assets/3da88cab-1514-40f2-803f-8a0dded333e7" />

Hit create.

3. Went to DNS resource then > `Record sets` then noted NS record sets since all 4 are needed to update registrar:
```
ns1-05.azure-dns.com.
ns2-05.azure-dns.net.
ns3-05.azure-dns.org.
ns4-05.azure-dns.info.
```

4. Next created DNS record from the top. Added `wwww` as name, left everything else as default and added standard ip `10.10.10.10`:

<img width="545" height="528" alt="image" src="https://github.com/user-attachments/assets/f83a2340-64a8-4188-b410-89e01937494f" />

Verified record was created:

<img width="688" height="71" alt="image" src="https://github.com/user-attachments/assets/39606367-2786-4dc8-b087-4d507e786a6f" />

5. Next i needed to verify global AZ DNS

Even though i didnt have registered domain, it was still possible to verify the DNS zone worked using `nslookup`.

6. Opened up CLI and ran the following:
```bash
nslookup www.Mmerch.com ns1-05.azure-dns.com
```

Output:

<img width="554" height="168" alt="image" src="https://github.com/user-attachments/assets/df5c3693-c748-405a-aba5-3ec862bf77da" />

I now had successfully set up DNS zone and created an A record.

### Summary

Basically for this mini lab i set up a public DNS zone in Azure for a site called `Mmerch.com`. Once the zone was made, i saw that Azure gave me 4 different name servers to use. Went ahead and added an "A record" for `www` and pointed it to `10.10.10.10`. Since i dont actually own the domain `Mmerch.com` in real life i couldnt just test it the normal way. I had to use `nslookup` in the terminal and tell it to talk specifically to one of the Azure name servers i noted earlier. It worked perfectly and returned the IP i set, which proves the DNS zone is configured right.

### What i learned

* How to create a public DNS zone and add A records in the Azure portal.
* Azure gives you 4 name servers (com, net, org, info) for redundancy so your DNS stays up.
* The "A record" is what maps a name like `www` to a specific IP address.
*  You dont need a real domain registrar to test your work. You can verify your DNS records are correct just by forcing nslookup to query the Azure name server directly.
