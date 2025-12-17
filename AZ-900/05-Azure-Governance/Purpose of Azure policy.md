# Describe the purpose of Azure policy

## Table of contents
* [Azure policy](#azure-policy)
* [How does it define policies?](#how-does-it-define-policies)
* [Other important details](#other-important-details)
* [Azure policy initatives](#azure-policy-iniatives)

## My interpretations

## Azure policy

Is a service in Azure that enables you to create, assign and manage policies that control or audit your resources. These same policies enforce different rules across your resource configurations so that those configurations stay compliant with corporate standards.

### Why does it exist?

To ensure that all Azure resources follow organizational standards and remain compliant with security and business rules.

### What does it do?

It enforces rules on resource configurations allowing you to block non compliant creations, audit existing ones or automatically fix certain settings.

### How does it work?

It compares resource properties against policy definitions (JSON rules) at deployment time and on a daily schedule, applying "effects" (like Deny or Audit) based on the result.

### Summary

Azure Policy is like a digital rulebook for your cloud. It makes sure everyone builds things correctly and automatically warns or stops you if you try to break the rules.

## How does it define policies?

### Why does it exist?

To provide a scalable way to group and enforce rules/policies (initiatives) across different organizational levels to prevent or fix misconfigurations.

### What does it do?

It manages individual rules or "initiatives" (groups of policies), identifies non-compliant resources, blocks illegal deployments and automatically fixes (remediates) missing settings.

### How does it work?

Policies are assigned at specific levels (Resource, Resource Group, Subscriptions etc) and are inherited by all children. They check both new and existing resources and can even integrate with DevOps pipelines.


### Summary

Its a system of "smart rules" that pass down from top folders to everything inside. It flags errors, stops you from making mistakes or fixes them for you unless you mark an exception.

## Other important details

* Built in templates: Azure provides ready to use policy definitions for common services like storage, networking and compute.
* Retroactive scanning: It doesnt just check new resources, it constantly monitors  and evaluates existing ones created before the policy existed.
* Exclusions: You can manually flag specific resources as exceptions if you dont want the policy to apply to them or fix them.
* DevOps integration: Policies can be linked to your deployment pipelines to catchi compliance before code even goes live.

Think of Azure policies as a smart filter  that comes with premade rules, it automatically passes rules down to all sub folders, checks your old work for mistakes and can be told to basically skip certain items if necessary.

## Azure policy initatives

Is a way of grouping related policies together.

### Why does it exist?

To simplify the management of multiple related policies by grouping them into a single unit centered around a specific goal.

### What does it do?

It tracks the aggregate compliance state of a collection of policies making it easier to monitor high level objectives like Security or Networking.

### How does it work? 

You define an Initiative definition that contains several Individual Policy Definitions. When assigned Azure evaluates all of them together.

### Summary

An initiative is like a rule playlist. Instead of checking 20 separate rules one by one you use a single list that covers a big topic like Safety Standards. Rules being Policies in this context.

Under this iniative the following policy definitions are included:

* Monitor unencrypted SQL Database in Security center: This monitors unencrypted SQL databases and servers.
* Monitor OS vulnerabilities in Security center: This monitors servers that dont satisfy the configured OS vulnerability baseline.
* Monitor missing Endpoint protection in Security center: This monitors for servers that dont have an installed endpoint agent.

**Important!**

Enable Monitoring in Azure security center initiative contains over 100 seperate policy definitions.
