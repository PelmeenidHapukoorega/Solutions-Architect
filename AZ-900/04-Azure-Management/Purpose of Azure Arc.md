# Describe the purpose of Azure Arc

## Table of contents
* [Azure Arc](#azure-arc)
* [What it can do outside of Azure](#what-it-can-do-outside-of-azure)

## My interpretations

## Azure Arc

Lets you extend your Azure compliance and monitoring to your hybrid and multi cloud configurations. It simplifies governance and management by delivering a consistent multi cloud and on prem management platform.

### Why does it exist?

To solve the complexity of managing hybrid and multi cloud environments where resources are scattered across on prem, AWS, GCP, and the edge.

### What does it do?

It provides a unified management platform that extends Azures security, compliance and monitoring tools to resources living outside of Azure.

### How does it work?

It uses Azure Resource Manager (ARM) to "project" non Azure resources into the portal as if they were native resources, typically using a local agent.

### Summary

Azure Arc is a bridge that lets you manage your outside servers and databases using the exact same Azure dashboard and rules you use for your inside ones.

Its a centralized unified way to:

* Manage your entire environment together by projecting your existing non Azure resources into ARM.
* Manage multi cloud and hybrid virtual machines, Kubernetes clusters and databases as if they are running in Azure.
* Use familiar Azure services and management capabilities, regardless of where they live.
* Continue using traditional ITOps while introducing DevOps practices to support new cloud and native patterns in your environment.
* Configure custom locations as an abstraction layer on top of Azure Arc enabled Kubernetes clusters and cluster extensions.

## What it can do outside of Azure

It allows you to manage the following resource types hosted outside of Azure:

* Servers
* Kubernetes clusters
* Azure data services
* SQL server
* VMs (preview)


