# Describe Cloud service types

## Table of contents
* [Benefits of high availability and scalability](#benefits-of-high-availability-and-scalability)
* [Benefits of reliability and predictability](#benefits-of-reliability-and-predictability)
* [Benefits of security and governance](#benefits-of-security-and-governance)
* [Benefits of managebility](#benefits-of-managebility)

## Describe Infrastructure as a Service

IaaS gives you remote access to the basic cloud building block like virtual machines, storage and networks over which you manage the OS and application.

### Why does it exist?

It provides organizations with the highest level of control and flexibility over their core computing resources without massive upfront cost and management burden over owning physical datacentres.

### What does it do?

Provides customers with virtualized fundamental computing resources over the internet. Basically it delivers raw components you need to build and run applications by abstracting away the management of the physical hardware.

### How does it work?

It works by allowing users to rent and remotely access fundamental computing resources like virtual servers, storage and networks over the internet. The cloud provider handles the physical side of things.

### Responsibility

In a IaaS scenario most of the responsibilty is on the user. Cloud provider gives you the datacentre and is responsible for the physical side of things and its internet connection, while you the user is responsible for installation and configuration as well as patching, updates and the security of it.

### Use case scenarios

* Lift and shift migration: You are setting up cloud resources just like you would on your own on prem datacentre and then simply moving the things running on prem to running on IaaS
* Testing and development: You have established configurations for development and test environments that you need to rapidly replicate. You can start up or shut down the different environments rapidly with IaaS while maintaining complete control.

### Summary

Basically IaaS as a service provides you with everything that a normal on prem datacentre does, just over the internet minimizing costs and giving the user more flexibility i.e renting a datacentre.

## Describe Platform as a Service

Platform as a Service or PaaS is the middle ground between renting soace in a datacentre (IaaS) and paying for a complete and deployed solution (SaaS).

### Why does it exist?

It gives developers a complete and ready to use environment for building, deploying and managing apps without the burden of managing the underlying infra like servers, OS systems and patching. It sits betweem IaaS which is raw infrastructure and SaaS which is finished software to maximize developer speed and efficiency.

### What does it do?

Provides developers with a complete and pre configured environment over the internet, allowing them to focus entirely on writing, deploying and managing application code without having to worry about infra itself.

### How does it work?

It works by abstracting away the operating system and infra management so developers only have to focus on their code.

### Responsibility

In PaaS scenario the responsibility is shared because its the middle ground. Like in IaaS the cloud provider is responsible for the physical side but it will also maintain the OS systems, databases and development tools provided to you. Its like having your own IT department who does regular updates, patches and refreshes.

### Use case scenarios

* Development framework: PaaS provides a framework that you as the developer can use to build upon to develop or customize cloud based applications using built in software components. Features such as high availability, scalability and multi tenant cababilities are included which helps reduce the amount of coding you would need to do.
* Analytics or business intelligence: Tools provied as a service with PaaS allow you to analyze and mine your data which helps finding insights, patterns and predicting outcomes to improve forecasting, design decisions, investment returns and other business decisions.

### Summary

PaaS is a combination between IaaS and SaaS. You are being provided with the physical datacentre as well as network connection, in addition you are being given complete and pre configured environment over the internet allowing you to focus on your code.


