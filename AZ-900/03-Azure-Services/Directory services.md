# Describe Azure directory services

## Table of contents
* [Microsoft Entra ID](#microsoft-entra-id)
* [Can you connect on prem AD with Entra ID?](#can-you-connect-on-prem-ad-with-entra-id)
* [Is information synchronized?](#is-information-synchronized?)

## My interpretations

## Microsoft Entra ID

Entra ID is a directory service that enables you to sign in and access both Microsoft cloud applications and cloud apps that you develop. It also helps you maintain your on prem Active Directory deployment.

### Why does it exist?

To provide a centralized, cloud based IAM solution for the modern, complex environment.

### What does it do?

It solves the problem that the traditional on prem directory services like Windows Server Active Directory were not designed to handle the shift to cloud and mobile computing.

* Authentication: Includes verifying identity to access apps and resources. Inlcludes SSO, MFA and custom list of banned passwords as wwell as smart lockout services.
* Single sign on: SSO enables you to remember only 1 username and 1 password to access multiple apps. Its tied toa user which simplifies the security model. As you would change roles within your organization, access modifications are tied to that identity, which reduces the need for effort to change or disable accounts.
* Application management: You can manage your cloud on prem apps with Entra ID. Features like Application proxy, SaaS apps the My Apps portal and SSO provide a better user experience.
* Device management: Entra ID supports the registration of devices, this enables devices to be managed through tools like Microsoft Intune, it also allows for device based conditional access policies to restrict access attempts to only those coming from known devices, regardless of the requesting user account.

### How does it work?

It works as a broker of trust between a user and the application they are trying to access, handling the authentication and authorization centrally. Meaning it acts as a trusted middleman in every connection between a user and cloud application.

### Summary

Microsoft Entra ID is IAM management service, in other words you control who gets to use what and who gets where. It can also maintain your on prem AD deployment. Entra ID works similar to AD directory so it feels familiar. It simplifies your life when interacting with Azure services and AD directory by making you remember fewer passwords, making access more secure automatically and letting you manage all the access rules from 1 central place.

## Who is it for?

* It administrators: You can use Entra ID to control access to applications and resources based on their business requirements.
* App developers: You can use Entra ID to provide standards-based approach for adding functionality to applications that you build, such as SSO functionality to an app or enabling app to work with users existing credentials.
* Users: Users can manage their identities and take maintenance actions like self service password reset.
* Online service subscribers: MS 365, MSO 365, Azure. Microsoft dynamics subscribers are already using Entra ID to authenticate their account.

## Can you connect on prem AD with Entra ID?

If you have on prem running AD and cloud deployment using Entra ID you would need to maintain 2 identity sets. But you can connect AD with Entra ID enabling consistent identity experience between both cloud and on prem. 

## Microsoft Entra Domain services

Microsoft Entra Domain Services provides managed domain services such as domain join, group policy, lightweight directory access protocol (LDAP) and Kerberos/NTLM authentication. Just like Entra ID lets you use directory services without having to maintain the infra supporting it.

### Why does it exist?

To solve a specific problem in cloud migration: Legacy application compatibility with a fully managed solution.

### What does it do?

It bringes the functionality of a traditional windows server active directory domain controller to Azure.

### How does it work?

It works by acting as a virtual, fully managed version of a traditional windows server domain controller that is synchronized with your cloud identity like Entra ID.

* You create managed domain, you define a unique namespace. This the domain name.
* 2 windows server domain controllers are then deployed into selected region. This deployment of DC is called replica set.
* No need to manage, configure or update these DC. Platform handles it, including backups and encryption at rest using Azure disk Encryption.

## Is information synchronized?

* Its configured to perform a one way synchronization from Entra ID to Entra domain services.
* In a hybrid environment, Entra connect synchronizes identity information wtih Microsoft Entra ID, which is then synced to the managed domain.

### Summary

In essence its like a fully managed adapter in the cloud that translates your Etra ID credentials so that old tradtional windows apps can still use familiar protocols like Kerberos and NTLM to authenticate.
