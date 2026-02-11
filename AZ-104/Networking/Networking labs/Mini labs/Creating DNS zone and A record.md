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

### What i learned
