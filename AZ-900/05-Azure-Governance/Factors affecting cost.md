# Describe factors that can affect costs in Azure

## Table of contents
* [CapEx](#capex)
* [OpEx](#opex)
* [Resource type](#resource-type)
* [Consumption](#consumption)
* [Maintenance](#maintenance)
* [Geography](#geography)
* [Network traffic](#network-traffic)
* [Subscription type](#subscription-type)
* [Azure marketplace](#azure-marketplace)

## My interpretations

Azure shifts development costs from the capital expense (CapEx) of building out and maintaining infra and facilities to an operational expense (OpEx) of renting infra as you need it, whether its compute, storage, networking etc.

## CapEx

### Why does it exist?

It exists because businesses need to make long term investments that provide value and benefits beyond a single year. Its a fundamental concept in business and accounting that recognizes the distinction between investing for the future and paying for day to day operations.

### What does it do?

Its a financial mechanism a company uses to acquire, improve and maintain assets that are expected to benefit the business for more than 1 year.

### How does it work?

It works by dictating how large, long lasting investments are treated financially. Its based on the idea that the cost should be recognized over the assets useful life not all at once.

### Summary

CapEx in simple terms is, spend more now to future proof and spend less later. Its a longterm investment before actually seeing if it pays off.

## OpEx

### Why does it exist?

It exists because companys need money for daily survival and accountants need a simple timely way to calculate annual profit and taxes. So it needs to be done in real time, to make sure annual reports are accurate.

### What does it do?

Its basically the engine money that covers everything consumed this year to generate revenue and its immediately written off for tax purposes.

### How does it work?

It works by being a simple immediate deduction that flows directly through the companys financial statements to determine annual profit and taxes.

### Summary

OpEx in simple terms is is spend now because we need it now, its an immediate deduction that later simplifies how much was used either quarterly for quarterly reports or annualy for annual reports.

OpEx cost can be impacted by following factors:

* Resource type
* Consumption
* Maintenance
* Geography
* Subscription type
* Azure marketplace

## Resource type

### Why does it exist?

It exists to determine how much a resources costs by ensuring different factors such as type of resource, its specific settings and the Azure region all have an impact on the final cost. Its the system that generates the usage record used to calculate your bill.

### What does it do?

It creates a metered instances for the Azure resource when the resource is provisioned. The meters then track the resources usage and then generate a usage record based on that tracking.

### How does it work?

When you provision resource, Azure creates metered instances for it, the meters then track the usage of those resources. Usage tracking then generates a usage record, this record is then used to calculate your bull, while the cost itself is influenced by the resource type, its settings and the region.

### Summary

Resource type factor determines how much do you have to pay for provisioning the resource based on type, time used, settings applied, locations used etc. 

Examples of factors that affect

**Storage account**

* Type
* Performance tier
* Access tier
* Redundancy settings
* Region

**Virtual machines**

* Licensing for OS
* Licensing for other software
* Processor
* Number of cores for VM
* Attached storage
* Network interface
* Region

## Consumption

Pay as you go has been consistent theme and thats the cloud payment model where you pay for resources that you use during a billing cycle.

### Why does it exist?

Consumption exists to align cost with usage. The existance of this model is to move away from rigid CapEx and allow companies to pay only for resources they actually consume aka OpEx. It gives you financial flexibility with pay as you go model and also provides reservation option to reward users with discounts up to 72% on the condition that you can commit to a consistent, set amount of resource usage over a long period (1-3 years)

### What does it do?

Ensures that you only pay for what you use during a billing cycle. More compute used = pay more and vice versa. It also locks in committed amount of resources at much lower discounted rate, allows you to recognize savings on stable predictable workloads and combines PAYG to provide safety net for any usage that exceeds the reserverd capacity.

### How does it work?

When a resources is used Azure creates meters that track its usage, meters generate record and then are calculated into your final bill.

### Summary

Consumption consists of both PAYG and Reservations, this means that you only pay for what you use, and additionally it helps you recognize savings on stable predictable workloads.

## Maintenance

### Why does it exist?

Need for maintenance exists explicitly to control cloud costs by preventing unnecessary spending. It addresses the problem that occurs when a main resource like VM is deprovisioned but additional resources like say storage or networking may remain provisioned, leading to continued wasted charges. This is where resource group comes in handy by keeping all of your resources organized which makes cost control and cleanup easier.

### What does it do?

Enables rapid resource adjustment, meaning it leverages the clouds flexibility to rapidly adjust resources based on demand. Using RG groups helps to keep resources organized. By ensuring you are not keeping around resources that are no longer needed, it helps you to control cloud costs.

### How does it work?

You use RG groups to organize associated resources logically. Observe your resources to identify when they are no longer needed. Then actively ensure that all associated resources are deprovisioned when the main resource is deprovisioned, especially those that may not disappear automatically.

### Summary

Maintenance in this context means its wise to hold resources in RG groups so its easier to deprovision them without running into risk that subsequent resources are still operational which creates usage and adds to your bill. Additionally you need to keep an eye on your resources to distinguish which ones are actually relevant and which ones are no longer needed.

## Geography

### Why does it exist?

To reflect global cost variations, it exists because the underlying real world costs of running cloud infra, such as power, labor, taxes and fees naturally vary depending on the location (Azure region). It provides necessary mechanism to allow customer to deploy their services centrally or closest to their customers by leveraging Azures globally distributed infra.

### What does it do?

Determines resource pricing, it directly causes resources to differ in costs to deploy depending on the region. It impacts the cost of moving data, specifically it makes network traffic less expensive within a large area than between continents. It also forces you to define a region where the resources will be depoliyed when you provision most resources.

### How does it work?

Azures infra is distributed globally, due to the variations in local expenses, Azure applies global pricing differences to its resources based on the specific region.

### Summary

Geography is important when it comes to picking a region, because region selection is in direct correlation to your costs, for example it makes network traffic less expensive within a large area than between continents, because traffic between continents requires more resources than within a region.

## Network traffic

### Why does it exist?

To bill for the resources required to move data, particulary for outbound data transfers which incurs costs. Billing zones are for grouping Azure regions geographically for billing purposes, making the pricing structure more manageable than charging for every single region to region transfer individually. The design includes some inbound data transfer for free, which can encourage you to move your data onto the Azure platform.


### What does it do?

Defines data movement as either inbound or outbound. Sets the data transfer pricing for outbound transfers based on zones. Zones serve as the geographical groupings of Azure regions used to apply the relevant bandwidth pricing.

### How does it work?

The system first distinguishes if the data is entering or leaving Azure. For outbound data the pricing is determined by which billing zone the data is moving from/to. The specific cost rates are detailed on the bandwidth pricing page, which provides additional information on pricing for data ingress, egress and transfer.

### Summary

Basically you pay for data moving OUT of Azure but not for data moving IN. Cost depends on how far the data travels so that means its less expensive to mova data within the Zone but more so across continents.

## Subscription type

Some subscription types include usage allowances, which affect costs. For example free tier provides a number of Azure products that are free for 12 months, while also including credit to spend within the first 30 days of sign up.

Subscriptions are also a good way to manage costs, by creating seperate subscription for say storage, networking, compute etc.

## Azure marketplace

Marketplace lets you purchase Azure based solutions and services from 3rd party vendors. Could be a server with software preinstalled and configured or managed network firewall appliances or even connectors to 3rd party backup services. 

When you purchase products through marketplace you may pay for not only services that you are using but also services or expertise of the 3rd party vendors. Billing is set by the vendors.

All solutions available on there are certified and compliant with Azure policies and standards.
