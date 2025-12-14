# Describe Azure RBAC

## Table of contents
* [RBAC](#rbac)
* [Additional info](#additional-info)
* [How is RBAC applied to resources?](#how-is-rbac-applied-to-resources)
* [How is RBAC enforced](#how-is-rbac-enforced)

## My interprentations

## RBAC

RBAC is the primary authorization system used to manage access to Azure resources.

### Why does it exist?

To enforce the fundamental security principle of Least Privileg across your entire cloud environment.

### What does it do?

Its an authorization system that determines what an authenticated identity is allowed to do on the resources you pay for.

### How does it work?

It works by combining 3 core elements into a single Role assignment, which is the mechanismt used to grant and enforce permissions on your Azure resources.

Core elements being:

* Security principal (the WHO)
* Role definition (the WHAT)
* Scope (the WHERE)

## Additional info

* Azure provides built in roles that describe common access rules for cloud resources.
* Enables you to control access through Azure RBAC by assigning roles with distinct policies over a defined scope.
* When you assign individuals or groups to 1 or more roles, they recieve all the associated access permissions.

## How is RBAC applied to resources?

RBAC is applied to a scope aka the WHERE, which in essence is a resource or resources that this access applies to.

[RBAC and scopes relationship](../02-Azure-Architecture/Diagrams/RBAC%20and%20scopes%20relationship.md)

Scopes include:

* Management group (collections of multiple subs)
* Single subscription
* Resource group
* Single resource

RBAC is hierarchical, in that when you grant access at a parent scope, those permissions are inherited by all child scopes. Examples:

* When you assign the Owner role to a user at management group scope, it gives the user the ability to manage everything in all subscriptions within the management group.
* When you assign the Reader role to a group at the subscription scope, the members of said group can view every resource group and resources within the subscription.

## How is RBAC enforced?

Its enforce of any action thats iniated against resources that passes through Resource manager.

Resource manager is management service that provides a way to organize and secure your cloud resources. You access it usually from the portal, cloud shell, powershell and the CLI.

RBAC doesnt enforce access permissions at the app or data level. App security must be handled by your app.

RBAC uses an allow model, meaning that when you are assigned a role RBAC allows you to perform actions within the scope of that role. If 1 role assignment grants you read permissions to a resource group and a different role grants you write permissions to the same resource group, you have both read and write within that resource group.

### Summary

RBAC is used to control who has access where within the scope, its a good way to structurize department and individual roles without giving full access to everyone for everything. It allows you to govern quite well over multiple resources at once or over singular resource, depending on what you want key individuals or departments to have access to.
