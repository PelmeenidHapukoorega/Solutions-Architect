# Describe the purpose of resource locks

## Table of contents
* [Resource lock](#resource-lock)
* [Types of resource locks](#types-of-resource-locks)

## My interpretations

## Resource lock

A resource lock prevents resources from being accidentally deleted or changed.

### Why does it exist?

To prevent accidental deletion or modification of critical resources even by users who have the administrative permissions (RBAC) to do so.

### What does it do?

It restricts specific actions on resources either preventing deletion (CanNotDelete) or making them read only (ReadOnly) to prevent any changes.

### How does it work?

Locks are applied at the resource, resource group or subscription level. They are inherited meaning a lock on a folder protects everything inside it.

### Summary

A resource lock is like a safety cover over a button. It ensures that even if you have the keys to the system you cant accidentally delete or change important parts without removing the lock first.

## Types of resource locks

There are 2 types of resource locks. On prevents from deleting and the other prevents from changing or deleting.

* Delete: Means authorized users can still read and modify a resource however they cant delete it.
* ReadOnly: Means authorized users can read a resource but they cant do anything else like dele or update it. Its similar to restricting all authorized users to the permissions granted by the reader role.

## How to manage resource locks

You can either manage them from:

* Azure portal
* Powershell
* Azure CLI
* Azure resource manager template

For portal you can view, add or delete locks under the Locks section of any resource settings pane.

## How to delete or change locked resource

To modify locked resource you need to remove the lock first. After the lock removal you can apply any action you have permissions to perform. 

Resource locks apply regardless of RBAC permissions. You can be the owner of resource but you still have to remove the lock before you can perform any changes to resource.
