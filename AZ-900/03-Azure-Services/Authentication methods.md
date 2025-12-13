# Describe Azure authentication methods

## Table of contents
* [SSO](#sso)
* [Multifactor authentication MFA](#multifactor-authentication-MFA)
* [Microsoft Entra multifactor authentication](#microsoft-entra-multifactor-authentication)
* [Passwordless authentication methods](#passwordless-authentication-methods)
* [Windows Hello for business](#windows-hello-for-business)
* [Microsoft authenticator app](#microsoft-authenticator-app)
* [FIDO2 security keys](#fido2-security-keys)

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

## Microsoft Entra multifactor authentication

Is a Microsoft service that provides multifactor authentication capabilities.

### Why does it exist?

To provide the most effective defense available against credential theft and unauthorized cloud access, serving as the essential security foundation for Microsofts identity platform.

### What does it do?

It hardens the logic gateway to all cloud apps, dynamically challenging users based on risk and policy to prevent the vast majority of identity based attacks.

### How does it work?

By intercepting the standard sign in process and injecting a mandatory, real time security check that relies on the user providing proof from 2 different factors.

### Summary

Entra multifactor authentication is part of MFA in general, it acts as the security side of MFA, meaning that it provides defense against potential credential theft and unauthorized access.

## Passwordless authentication methods

Passwordless authentication methods are more convenient because the password is removed and replaced with something you have, something you are or something you know. It needs to be set up on a device before it can work.

## Windows Hello for business

Windows hello for business is a robust enterprise grade authentication method from Microsoft that allows users to sign in to their Windows devices and access corporate resources without using a traditional password.

### Why does it exist?

To solve the fundamental enterprise problems of password vulnerability, user friction and decentralized identity management by delivering a phishing resistant, passwordless authentication solution.

### What does it do?

It uses security of a cryptographic key combined with the convenience of a PIN or biometric to makinge logging into the network or cloud services easier, safer and centrally manageable.

### How does it work?

In essence it replaces the password base login with a secure, cryptographic key based handshake.

### Summary

Basically windows hello for business is like replacing your physical house key with a highly secure, uncopyable digital key thats stored in a safe, which you unlock using only your fingerprint or a simple secret PIN.

## Microsoft authenticator app

Is a convenient multifactor authentication option in addition to a password.

### Why does it exist?

To provide a highly secure, convenient and managed in 1 place mobile platform for users to perform phishing resistant, multi factor and passwordless authenticaion to all their online accounts.

### What does it do?

The app is central, secure and convenient repository for all the secondary verification factors needed to protect a users digital identity.

### How does it work?

It works by acting as a secure, physical validator that proves you have a registered device, allowin it to complete the second step of 2 step login.

### Summary

Microsoft authenticator app is basically like, say you wanna go to the clubs VIP section, you enter the club aka sign in so to speak, but then the bouncer says that in order to get into VIP section you need to have a special invitation. This is authenticator app in a nutshell.

## FIDO2 security keys

FIDO or Fast IDentity Online helps to promote open authentication standards and reduce the use of passwords as a form of authentication.

### Why does it exist?

To solve the vulnerability of passwords and one time codes to phishing and Man in the Middle or MITM attacks.

### What does it do?

It acts as a portable, hardware vault that holds your unphishable login proof and only releases it to the specific websites for which it was registered.

### How does it work?

It replaces the traditional password login with an unstealable, hardware secured digital signature using public key cryptography.

### Summary

FIDO2 is like tamper proof USB identity stamp that unlike a password, can only digitally sign a transaction for the correct bank website, making them completely useless to a thief on a fake website. In essence it works quite similar to Smart ID which is eID platform, the difference being in hardware. 
