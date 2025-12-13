# Describe Azure authentication methods

## Table of contents
* [SSO](#sso)
* [Multifactor authentication MFA](#multifactor-authentication-MFA)

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

**Important!**

SSO is only as secure as the inital authenticator because the subsequent connections are all based on the security of the inital authenticator.

## Multifactor authentication MFA

Is the process of prompting user for an extra form or factor of identification during the sign in process.

### Why does it exist?

It exists to protect accounts from compromise by requiring a secondary verification factor, making stolen passwords useless to attackers.

### What does it do?

It performs the core function of verifying a users identity by requiring proof from 2 or more distint categories (think password and a phone)

### How does it work?

It works by requiring a sequential, 2 stage verification process that must successfully pass 2 or more independent factors before granting access.

### Summary

Essentially MFA is almost like SSO with extra steps, instead of giving you access straight away MFA makes you jump through hoops. Example, you type in your password and login, then the app requires you to type in a code that was sent on your secondary email, or you have an app for the authentication code required before MFA accepts full authorization to let you in.
