# Describe Azure management infrastructure

## Table of contents
* [Management infrastructure](#management-infrastructure)
* [Resources](#resources)
* [Resource groups](#resource-groups)
* [Subscriptions](#subscriptions)
* [Management groups](#management-groups)

## Management infrastructure

Management infra refers to the tools, processes and technologies withing the cloud that organizations deploy on the platform

### Why does it exist?

It existst to tame complexity i.e make deployments and management easier, ensure security and control the costs of the organizations cloud resources. It transforms the raw and distributed cloud capacity into a governable, compliant and efficient operational environment.

### What does it do?

It serves to govern, automate and secure the computing resources that organization deploys on the cloud platform.

### How does it work?

By using integrated software tools and application programming interface i.e API to communicate with and control resources.

### Summary

Simply put, if you dont have the tools then how exactly can you build or do anything?

## Resources

Resources are the fundamental components of computing, storage and networking that organizations rent on a pay as you go basis from cloud provider.

### Why does it exist?

Because they solve the problems of cost, scalability and complexity that is usually associated with owning and managing physical infrastructure.

### What does it do?

It uses integrated software tools and API to communicate and control resources of the public cloud provider.

### How does it work?

Resources work by being provisioned and controlled through API which then transforms raw physical infra into scalable on demand services.

### Summary

Resources are like physical components and hardware used in physical on prem data centers but virtualized. Aka building blocks.

## Resource groups

Resource group is a grouping of different or the same resources. When you create a resource you are required to place it into a resource group. Resource group can contain many resources however a single resource can only be in one resource group at a time. If you move the resource into a different group, it will no longer be associated with the former group.

### Why does it exist?

It provides a logical container for organizing, managing and governing all the individual resources that belong to a single application, project or environment.

### What does it do?

It orgnaizes and governs multiple individual cloud resources that are related to single app or project that are easily managable within the container.

### How does it work?

It creates a logical boundary within the cloud management system where policies, access control and lifecycle commands are applied to all contained resources at the same time as long as they exist in the same resource group/

### Summary

Resource group is a logical container where you can stack different resources, you can add policies to the resource group and all the resources inside the group will adopt the policy thus removing the need to seperately apply policies to each individual resource. Its a lot easier to manage and organize your resources.

## Subscriptions

Subscriptions are a unit of management, billing and scale. Similar to resource groups logically. Subscriptions help you logically organize resource groups and facilitate billing.

### Why does it exist?

It provides a high-level administrative, billing and resource governance boundary that ties together the identity, costs and resources used by either the user, a specicic customer, department or a project.

### What does it do?

Its the highest administrative container for managing bills, access control and service limits across all the cloud resources an organization consumes.

### How does it work?

By acting as the primary administrative account that provides the financial identity and governance foundation for all resources deployed within the boundary.

### Boundaries

* Billing boundary: Subscriptions determine how an Azure account is billed for using Azure. You can create multiple subs for different types of billing requirements. Azure then generates a seperate billing reports and invoices for each subscription so that you can better organize and manage costs.
* Access control boundary: Azure applies access management policies at the subscription level and you can create seperate sub to reflect different organizational structures. For example, you have different departments in your business to which you apply distinct subscription policies. This model allows you to manage and control access to the resources that users provisions with specific subscriptions i.e a filter of sorts to control who gets to deploy what resources.

### Summary

Subscriptions are used for billing and also governance, you can say build an IaaS for someone under a specific subscription and then set up the billing for the customer who ordered the infra. Its a good method to seperate who has to pay for what project, app or service.

## Management groups

Management group is a governance structure that sits above the subscription level.  The hierarchy is: Management groups > Subscriptions > Resource groups > resources.

### Why does it exist?

It exists because it provides a framework to better organize multiple subscriptions into a hierarchy, allowing for large scale policy, access and compliance management across an entire organization.

### What does it do?

It acts as a governance hierarchy layer above subscriptions. It allows for better management of access, policies and compliance for multiple subscriptions at the same time from a central point.

### How does it work?

It works by creating a hierarchical tree structure where rules applied at a higher level automatically inherit and cascade down to all lower level subscriptions and the resources they contain.

### Summary

Management groups are the highest level of governance with in the cloud, every rule or policy you apply it to will cascade down. Only apply policies at the management level if they are non negotiable, enterprise wide security or compliance requirement because if you add the wrong one, everyone gets it.

### Important facts about management groups

* 10,000 management groups can be supported in a single directory.
* Management group tree can support up to six levels of depth. This limit doesnt include root level or the subscription level.
* Each management group and subscription can support only one parent.
