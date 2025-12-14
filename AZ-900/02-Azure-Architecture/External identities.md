# Describe Azure external identities

## Table of contents
* [External identities](#external-identities)
* [Microsoft Entra External ID](#microsoft-entra-external-id)
* [Capabilities of Entra External ID](#capabilities-of-entra-external-id)

## My interprentations

## External identities

An external identity is a person, device, service etc that is outside your organizations. 

## Microsoft Entra External ID

It refers to all the ways you can securely interact with users outside your organizations.

### Why does it exist?

To solve a challenge of securely managing access for every user who is not a direct employee within an organizations internal workforce.

### What does it do?

Its a unified platform for all non employee identity management, covering both partners B2B and consumers B2C while applying robust security policies.

### How does it work?

It works by establishing trust relationship with the external users identity, which allows them to access your resources without needing a full employee account.

### Summary

Microsoft Entra external ID provides access to people you do business with, be it either partners or consumers, its a good way to provide access to resources you would want to share with either one without needing to make them a business identity, which if you were to could be problematic, because then you are giving direct access to everything inside your services or apps.

## Capabilities of Entra External ID

* B2B collaboration
  * You can let partners use their preferred identity to sign in to your applications, they are represented in your directory typically as guest users.
* B2B direct connect
  * Establishes a mutual 2 way trust with another Entra organization for seamless collaboration. Currently supports Teams shared channels. Users are not represented in your directory, but they are visible within the Teams shared channel and can be monitored in Teams admin center reports.
* Microsoft Azure Active Directory B2C
  * Publish modern SaaS apps or custom developed apps to consumers and customers, while using Azure AD B2C for identity and access management.
