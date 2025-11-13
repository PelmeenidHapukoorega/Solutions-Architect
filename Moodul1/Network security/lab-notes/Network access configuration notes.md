# Module: Describe the core architectural components of Azure
# Configuring network access for installed VM and accessing web server - Lab Notes

Goal: In this exercise I needed to configure network access to the Virtual machine I had created prior in the last exercise.

**Note:** This exercise required me to use BASH version of cloud shell for some of the commands.

Task 1: Accessing my web server

1. For starters I need to access web server that I had installed prior to this exercise. To start things off I used ```az vm list-ip-addresses``` command to get my VMs IP address and store the result as a Bash variable:
```bash
IPADDRESS="$(az vm list-ip-addresses \
--resource-group "MinuVirtukas" \
--name my-vm \
--query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
--output tsv)"
```
2. Ran into the error [error.png](../screenshots/error.PNG), instead of ```--resource-group``` I miss spelled it in my haste to ```--resource-grp```

After fixing my GRAVE error the command went through [fix.png](../screenshots/fix.PNG)

3. Now I needed to download the home page to see if the VM is reachable and whether I could access the webserver (Spoiler alert it wasnt and I couldnt for obvious reasons):
```bash
curl --connect-timeout 5 http://$IPADDRESS
```
