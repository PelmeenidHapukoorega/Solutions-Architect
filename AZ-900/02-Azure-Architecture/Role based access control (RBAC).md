# Describe Azure RBAC

## Table of contents
* [RBAC](#rbac)
* [Additional info](#additional-info)
* [How is RBAC applied to resources?](#how-is-rbac-applied-to-resources)

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
