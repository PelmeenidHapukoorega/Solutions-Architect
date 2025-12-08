# Describe Virtual machines

## Table of contents
* [Virtual machines](#virtual-machines)
* [Availability sets](#availability-sets)
* [Azure virtual desktop](#azure-virtual-desktop)

## My interpretations

## Virtual machines

VMs, or virtual machines provide IaaS in the form of a virtualized server and can be use ind multitude of ways. Just like a physical computer, you can customize all of the software running on your VM. Ideal choice when you need total control over OS, ability to run custom software, use custom hosting configurations.

### Why does it exist?

VMs are fundamental flexibile units of computing power, that allow cloud providers to share massive physical resources among thousands of customers efficiently and cost effectively.

### What does it do?

They act as isolated and on demand computer systems that run your software seperate from the physical hardware they share.

### How does it work?

VMs work through a process called virtualization which is the process of using specialized software called "hypervisor" to carve up a single, powerful physical server into multiple isolated and independent virtual computer systems.

### Summary

Virtual machines are fundamental building blocks for building raw infrastructure. Think of a virtual machine like a computer that you download and manage software on.

## Scale VMs in Azure

You can either run single VMs for testing, minor task or development tasks, or you can group them together to provide high availability, scalability and redundancy. In Azure you can also manage the grouping of VMs with features such as scale sets and availability sets.

## Availability sets

Virtual machine availability sets are a tool to help you build a more resilient and highly available environment. They are designed to ensure that VMs stagger updates and have a varied power, network connectivity preventing you from losing all your VMs with a single network or power failure.

### Why does it exist?

To provide local redundancy for VMs within a single data center thus ensuring that apps remain available even when hardware fails or maintenance event occurs

### What does it do?

Availability sets distribute your VMs across seperate physical hardware within a single data center to ensure high availability for your apps.

### How does it work?

By using providers underlying control plane to group VMs and then automatically enforce the distribution of those VMs across distinct physical failure boundaries within a data center.

### Summary

Availability sets is like duplication of all your VMs in different places. It distributes your VMs automatically across seperate physical servers within a data center to create high availability and redundancy. Its like having your own real estate agent that picks you real estate in different locations to ensure variety. I.e you dont want to have your virtual machines all in 1 place, because if an outage occurs, all of that is lost.

### VM resources

* Size (purpose, number of processor cores, amount of RAM)
* Storage disks (HDD, SSD, etc)
* Networking (virtual network, public IP address, port configuration)

## Azure Virtual Desktop 

Virtual desktop is a type of virtual machine. Its a desktop and application virtualization service that runs on the cloud. It enables you to use cloud hosted version of windows from any location. It works across devices and operating systems and works with apps that you can use to access remote desktops or most modern browsers.

### Why does it exist?

To provide organizations with a secure, scalable and cost effective cloud based solution that delivers a virtualized desktop and applications.

### What does it do?

Delivers a full windows desktop and corporate applications to users on any device anywhere.

### How does it work?

It works by seperating the desktop experience from physical hardware, using the cloud to host actual computing resources and a microsoft managed control plane to manage all user connections and sessions.

### Emhanced security

AVD provides centralized security management for users desktops with microsoft Entra ID. You can enable MFA to secure user sign ins. You can also assign RBACs to users. With AVD the data and apps are seperated from local hardware, it all runs in the cloud which minimizes the risk of confidential data being left on a personal device. Additionally user sessions are isolated in both single and multi session environments.

### Summary

Basically AVD is like having a real pc which you could use extremely well for hybrid or remote work. It uses MFA or RBAC for security which minimizes risk of confidential data being left on personal devices. You spin up AVD and you can get to work, no need to download extra apps or set up personal devices. Essentially its the modern way of working. 
