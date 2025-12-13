# Describe Azure authentication methods

## Table of contents
* [Benefits of Azure DNS](#benefits-of-Azure-DNS)
* [Reliability and performance](#reliability-and-performance)
* [Security](#security)
* [Ease of use](#ease-of-use)
* [Customizable VNets with private domains](#customizable-vnets-with-private-domains)
* [Alias records](#alias-records)

## My interpretations

Authentication is the process of establishing the identity of a person, service or a device.

### Why does it exist?

Because its the fundamental gatekeeper of security and access in both digital and physical workloads.

### What does it do?

Authentication peforms 1 primary and critical function: verifying a claimed identity. In other words it veryfies that whoever wants to have access needs to be approved in the first place.

### How does it work?

It works by having the system compare the "proof" you provide against the security identity records it has stored.

### Summary

Authentication makes sure that only registered users allowed to access apss or data can do so. Its a protection layer.

[Security vs convenience](./Diagrams/Security%20vs%20convenience.md)

## SSO

SSO aka single sign on enables user to sign in 1 time and use credential to access multiple resources and apps from different providers.

### Why does it exist?

To solve a problem of password chaos and the resulting compromises both security and user efficiency. It was made to be the bridge between the need for high security and the demand of seamless user experience.

### What does it do?

It authenticates a user once and reuse that authentication across multiple independent applications.

### How does it work?

It works by creating a chain of trust between the user, the central identity system and the app the user wants to access. 
Its managed by 2 main entities using secure communication protocols (often SAML or OpenID).

### Summary

In short SSO performs the function of sharing a single verified identity securely across an ecosystem of apps, moving the focus from many individual passwords to 1 highly protected authentication process.

