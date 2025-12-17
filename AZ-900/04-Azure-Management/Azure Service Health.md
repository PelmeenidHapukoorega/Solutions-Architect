# Describe Azure Service Health

## My interpretations

## Azure Service Health

Keeps you on track of Azure resource, both your specifically deployed resources and overall status of Azure.

### Why does it exist?

To remove the mystery of cloud outages by providing a personalized view of how the health of the global Azure platform affects your specific apps.

### What does it do?

It tracks 3 levels of health: Global status (general issues), Service health (issues in the regions you use), and Resource health (the status of your specific VMs or DBs).

### How does it work?

It displays real time incidents, planned maintenance and health advisories in a dashboard and sends proactive alerts (Email/SMS) if your resources are impacted.

### Summary

Its your Cloud Weather Forecast. It warns you if a storm (outage) is hitting the specific neighborhood (region) where your servers are living.

The 3 services:

* Azure Status: is a broad picture of the status of Azure globally. Azure status informs you of service outages in Azure on the Azure Status page. The page is a global view of the health of all Azure services across all Azure regions. Its a good reference for incidents with widespread impact.
* Service Health: provides a narrower view of Azure services and regions. It focuses on the Azure services and regions you are using. This is the best place to look for service impacting communications about outages, planned maintenance activities, and other health advisories because the authenticated Service Health experience knows which services and resources you currently use. You can even set up Service Health alerts to notify you when service issues, planned maintenance, or other changes may affect the Azure services and regions you use.
* Resource Health: is a tailored view of your actual Azure resources. It provides information about the health of your individual cloud resources, such as a specific VM instance. Using Azure Monitor you can also configure alerts to notify you of availability changes to your cloud resources.
