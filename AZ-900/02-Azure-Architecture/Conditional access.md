# Describe Conditional access

## Table of contents
* [External identities](#external-identities)
* [Microsoft Entra External ID](#microsoft-entra-external-id)
* [Capabilities of Entra External ID](#capabilities-of-entra-external-id)

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

