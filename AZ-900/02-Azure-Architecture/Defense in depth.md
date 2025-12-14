# Describe defense in depth

## Table of contents
* [Defense in depth](#defense-in-depth)
* [Layers of DiD](#layers-of-did)
* [Physical security](#physical-security)
* [Identity and access](#identity-and-access)
* [Perimeter](#perimeter)
* [Network](#network)
* [Compute](#compute)
* [Application](#application)
* [Data](#data)

## My interprentations

## Defense in depth

The objective of defense in depth or DiD is to protect information and prevent it from being stolen by those who arent authorized to access it.

### Why does it exist?

It exists because no single security control is ever 100% effective, and you have to prepare for the inevitability of human error or a successful breach.

### What does it do?

It functions in a way that it ensures your organizations assets are protected by multiple, redundant and overlapping security controls.

### How does it work?

It works by creating a series of protective, independent and overlapping layers throughout the entire environment. The goal is to maximize the hurdles an attacker must overcome, ensuring that the failure of any single security mechanism does not lead to a complete security breach.

## Layers of DiD

Try to visualize DiD as a set of layers, with the data to be secured at the center and all the other layers functioning to protect the central data layer.

The layers are as follows in order:

* Physical security: Protection of computing hardware in a data center.
* Identity and access: Controls access to infra and change within the infra.
* Perimeter: Uses DDoS protection to filter large scale attacks before they can cause denial of service for users.
* Network: Limits communication between resources through segmentation and access control.
* Compute: Secures access to VMs.
* Application: Helps ensure that apps are secure and free of security vulnerabilities.
* Data: Controls access to business and customer data that you need to protect.

These help you provide a guideline to make security configuration decisions in all of the layers of your apps.

Each of these layers provides protection so that if 1 is breached a subsequent layer is already in place to prevent further exposure. This removes the need to rely on a single layer for protection. Overall it slows down an attack and provides alert info that security teams can then act upon either automatically or manually.

## Physical security

Physically securing access to buildings and controlling access to computing hardware within the data center are the first line of defense.

### Why does it exist?

Its the foundational layer in DiD model, it exists because all digital security measures are useless if a malicious actor gains direct, physical access to your hardware or infra that is stored within a data center.

### What does it do?

Its responsible for protecting the physical access to the hardware, infra and the physical location where your data resides.

### How does it work?

It works by creating a series of concentric, independent and increasingly restrictive physical barriers around the critical assets.

### Summary

Physical security layer is essentially provided by cloud provider itself since they are the host of the data centers where all the resources, infra and apps genuinely reside. Its essential to have protection for data centers because if anyone could be let in there freely then what exactly can stop an individual from tampering with the hardware.

## Identity and access

Its all about ensuring that identities are secure and access is granted only to whats needed and that sign in events and changes are logged.

### Why does it exist?

Because identity is the single most common and vulnerable entry point for sophisticated cyberattacks, especially in modern cloud and remote work environment. It serves as the gatekeeper layer, controlling who or what machine is allowed in and what they are allowed to do.

### What does it do?

It performs the essential functon of a digital gatekeepr and permission enforcer. It ensures that the entire system starts with a strong, verified foundation of trust before any resources are accessed.

### How does it work?

It works by ensuring that trust is only granted after successful, continous verification and only for a limited scope of actions. It follows a 3 step process for every access attempt:

* Authentication (prove who you are)
* Authorization (define what you can do)
* Policy enforcement and continous validation

### Summary

In short the IAM layer works to ensure that only the right identities are granted the correct permissions to the right resources at the right time.

## Perimeter

Network perimeter protects network based attacks against your resources. Identifying these attacks, eliminating their impact and alerting you when they happen are important ways to keep your network secure.

### Why does it exist?

It exists for a crucial historical reason, to act as the first digital line of defense and filter the massive volume of traffic between the untrusted external world meaning the internet and the trusted internal network. Its a gatekeeper for all data entering and leaving the organization.

### What does it do?

It performs the vital function of filtering, inspecting and controlling all data traffic at the boundary between your organizations trusted network and the untrusted external network. It acts as athe first digital checkpoint that every packet of data must pass through.

### How does it work?

It works by deploying specialized security devices and software at the network boundary to analyze, filter and control all data traffic, creating the first digital obstacle for external threats. Its less of a single wall and more of a complex monitored filtering station.

### Summary

Network perimeter is something where all data packets have to go through that means both inbound and outbound traffic, and what it does is that it filters and monitors data packets. If it seems something as a threat it flags it and removes it.

**At this layer its important:**

* To use DDoS protection to filter large scale attacks before they can affect the availability of a system for users.
* Use perimeter firewalls to identify and alert on malicious attacks against your network.

## Network

The focus on this layer is limiting the network connectivity across all your resoueces to allow only whats required. By limiting this communication you reduce the risk of an attack spreading to other systems in your network.

### Why does it exist?

It provides internal containment and control over the network environment. In essence it ensures internal resistance, if the attacker gets in they hit a series of internal, walled off compartments rather than open playing field.

### What does it do?

It reduces the risk of an attack to other systems in your network.

### How does it work?

It works by performing functions of containment, control and internal monitoring. Its purpose is to assume that external perimeter has failed and prevent an attacket from achieving their ultimate goal.

### Summary

Think of this layer as the layout for historical castles, network perimeter is basically the huge wall surrounding the castle, and then inside there you will have smaller wall sections. You break through the first big wall you get access to small village houses where peasants live, if you break down the next wall you get traders, blacksmith shops etc, the next wall is what guards the keep itself, if you break through that then all is lost.

**At this layer its important:**

* Limit communication between resources.
* Deny by default.
* Restrict inbound internet access and limit outbound access where appropriate.
* Implement secure connectivity to on prem networks.

## Compute

The focus on this layer is on making sure that your compute resources are secure and that you have the proper controls in place to minimize security issues.

### Why does it exist?

It exists soley to protect the actual running programs and the OS systems that host your data. Even if the attacker gets past the network and identity defenses, this layer is the final checkpoint before they can execute malicious code or access the data aka the treasure itself.

### What does it do?

It performs security controls at the closest point to your business logic, data and the running software itself. Its the final most specialized layer of defense that is designed to stop attacks that have successfully bypassed the network and identity controls.

### How does it work?

It works by implementing security controls and defenses directly on the OS and the apps running on your servers, think VMs, containers etc. Its focus is on hardening the environment and monitoring the code execution to prevent exploits from succeeding.

### Summary

Basically compute layer is the security that is built into the software itself and its immediate hosting environment, making it resilient against attacks even when an attacker successfully breaches the outer network walls. Basically in castle terms its the defense mechanisms within the castle, archers at the walls, oil pits, arrow slits, murder holes, battlements, drawbridges etc, everything to make it harder for the opposing army to get close to the keep.

**At this layer its important:**

* Secure access to VMs
* Implement endpoint protection on devices and keep systems patched and current.

## Application 

Integrating security into the application development lifecycle helps reduce the number of vulnerabilities introduced in code. Every development team should ensure that its apps are secured by default.

### Why does it exist?

It exists to protect the software itself from vulnerabilities that the network and identity layers cannot see or stop. Its the last most specific line of defense before the data itself.

### What does it do?

Protects the software, code and hosting environment that processes and holds your data. Like a treasure chest.

### How does it work?

It works by placing very specific security controls directly on the application code and the server it lives on, rather than just at the network edge. Its all about making the software resilient against flawed programming or malicious input.

### Summary

Basically application layer can be thought of as the entrance to the vault that keeps your treasure chest with the data inside the keep. Once you make past all the other security precautions youll get to the vault room which is heavily defended with more spikes, elite guards, traps on the vault door itself when opening etc.

**At this layer its important:**

* Ensure that apps are secure and free of vulnerabilities.
* Store sensitive app secrets in a secure storage medium.
* Make security a design requirement for all apps development.

## Data

Those who store data and control access to it are responsible for ensuring its properly secured. Often regulatory requirements dictate the controls and processes that must be in a place to ensure the confidentiality, integrity and availability of the data.

In almost all cases attacker are after data:

* Stored in a database.
* Stored on a disk inside VMs.
* Stored in SaaS apps such as Office 365.
* Managed through cloud storage.

### Summary

Basically if you are responsible for data and lets call it a treasure chest for now, then you are obligated to make sure the chest is made out of sturdy material, how many locks it has, where the keys are hidden, where the chest is place, who knows about it etc.
