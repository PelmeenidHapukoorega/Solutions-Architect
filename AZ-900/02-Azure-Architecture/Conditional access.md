# Describe Conditional access

## Table of contents
* [Conditional access](#conditional-access)
* [When to use?](#when-to-use)

## My interprentations

## Conditional access

Its a tool that Entra ID uses to allow or deny access to resources based on identity signals. These signals inclue who the user is, where the user is and what device the user is requesting access from.

### Why does it exist?

It exists because traditional security mode aka relying only on password and a static firewall, completely failed to protect organizations in the modern cloud and mobile world.

### What does it do?

It acts as the central policy enforcement point for all access to your cloud applications. Its primary function is to make an intelligent, adaptive access decision for every sign in attempt, based on a set of rulest that you define.

### How does it work?

By evaluating set of criteria in a real time during a users sign in attempt and then enforcing a specific outcome based on those criteria.

[Conditional access flow](../02-Azure-Architecture/Diagrams/Conditional%20access%20flow.md)


### Summary

Conditional access allows you to filter essentially who gets to use what resources and who doesnt, however it makes those decisions dynamically based on real time conditions and risk signals associated with the sign in attempt.

### Helps to

* Empower uses to be productive wherever and whenever.
* Allows you to protect the organizations assets.

## When to use?

**Useful when:**

* When you require MFA to access app depending on the requesters role, location, network.
  * Example: You could require MFA for admins but not regular users or for people connecting from outside your corporate network.
* When you require access to services only through approved client applications.
  * Example: You could limit which email apps are able to connect to your email services.
* When you require users to access your applications only from managed devices. A managed device is a device that meets your standards for security and compliance.
* Block access from untrusted sources, such as access from unknown or unexpected locations.
