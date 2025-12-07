# Describe containers

## Table of contents
* [Containers](#containers)
* [Container instances](#container-instances)
* [Container apps](#container-apps)
* [Azure kubernetes service](#azure-kubernetes-service)

## My interpretations

## Containers

Containers are lightweight, portable and efficient way to package applications code with all its dependencies like libraries, configuration files and system tools so it can run quickly and reliably across any computing environment.

### Why does it exist?

To solve the problem of inconsistent environments and the need for faster and efficient scaling than traditional VMs.

### What does it do?

They package and application with all its dependencies for guaranteed consistency by isolating it from other applications all the while sharing the host OS kernel which then enables rapid efficient scaling because they are lightweight and start pretty much instantly.

### How does it work?

It works by hosting OS kernel features to create isolated compartments for apps, rather than virtualizing the entire hardware like VM.

### Summary

Basically containers guarantee that your application will run whether the version is outdated, or incompatible with the platform, it ensures there are no conflicts when using the container regardless of versions. Its a good way to package your written code with a guarantee that no matter the version, it will run and it doesnt take any time at all to be deployed.

## Container instances

Containers instances offer fast and simples way to run a container in Azure. No need for management of virtual machines or adopt additional services to run it. PaaS offers container instances. It allows you to upload containers and then the service runs the containers for you. What you put in those containers is up to you.

## Container apps

Container apps are similar to instances. Fast deployment, removing the need for management and are PaaS offering. Additionally it has the ability to incorporate load balancing and scaling allowing for more elasticity with the design.

## Azure kubernetes service

AKS is an orchestration service, meaning it manages the lifecycle of containers. If you deploy a fleet of containers then AKS can make the fleet management more simple and efficient.

### Why does it exist?

It makes running and managing large numbers of containers easier by handling the complexities of maintenance for you.

### What does it do?

It focuses on automating the management and scaling of your containerized applications. Its like the invisible supervisor at work that makes sure your containers are running correctly, healing themselves when they fail and scale instantly when demand increases.

### How does it work?

It divides the container management responsibility between Azure and the user, allowing the user to focus only on whats important, the applications.

### Use cases of containers

Containers are mostly used to create solutions by using microservice architecture. This is where you break solutions into smaller and independent pieces. You can split containers in a way that 1 hosts the code for front end, another for back end and a third for storage. This in return allows you to seperate portions of your app into more logical and structured sections that can be easier to maintain, scale or updated independently.

### Summary

AKS is a supervisor that makes sure your containers are healthy and work as they are intended allowing you to focus on your applications. I.e management for containers.
