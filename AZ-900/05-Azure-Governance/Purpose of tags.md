# Describe the purpose of tags

## Table of contents
* [Purpose of tags](#purpose-of-tags)
* [How to manage resource tags](#how-to-manage-resource-tags)
* [Example tagging structure](#example-tagging-structure)

## My interpretations

## Purpose of tags

### Why does it exist?

To provide custom metadata (labels) on resources for better organization, management, and cost control as cloud usage scales.

### What does it do?

It classifies resources using key value pairs (tags) enabling resource location, cost grouping, operational planning, security classification and governance.

### How does it work?

You apply custom key/value text pairs to resources providing additional data that can be used later for filtering, reporting, and policy enforcement.

### Summary

Tags are custom labels you stick on resources to keep your cloud services organized, find them easily, and help you track spending by group.

This is useful for:

* Resource management tags: enable you to locate and act on resources that are associated with specific workloads, environments, business units, and owners.
* Cost management and optimization tags: enable you to group resources so that you can report on costs, allocate internal cost centers, track budgets, and forecast estimated cost.
* Operations management tags: enable you to group resources according to how critical their availability is to your business. This grouping helps you formulate service-level agreements (SLAs). An SLA is an uptime or performance guarantee between you and your users.
* Security tags: Enable you to classify data by its security level, such as public or confidential.
* Governance and regulatory compliance:  Tags enable you to identify resources that align with governance or regulatory compliance requirements, such as ISO 27001. Tags can also be part of your standards enforcement efforts. For example you might require that all resources be tagged with an owner or department name.
* Workload optimization and automation tags: Can help you visualize all of the resources that participate in complex deployments. Example: You might tag a resource with its associated workload or application name and use software such as Azure DevOps to perform automated tasks on those resources.

## How to manage resource tags

### Why does it exist?

To provide flexible methods for creating, updating, and enforcing organizational resource classification standards at scale.

### What does it do?

It allows you to manage (add, modify or delete) tags using various tools and enforce tagging rules across the environment using Azure policy.

### How does it work?

Tags can be managed via PowerShell, Azure CLI, ARM templates, REST API, or the Azure portal. Azure policy is used to automatically mandate or reapply specific tag conventions.

### Summary

You can change tags using common tools like the portal or scripts, and you use Azure policy to automatically ensure everyone uses the correct tags and follows the rules.

## Example tagging structure

<img width="908" height="300" alt="image" src="https://github.com/user-attachments/assets/9d6a0942-0448-4853-ae85-646274052d33" />

Keep in mind that you dont need to enforce that a specific tag is present on all your resources. Example: You might decide that only mission critical resources have the Impact tag. All non tagged resources would then not be considered as mission-critical.
