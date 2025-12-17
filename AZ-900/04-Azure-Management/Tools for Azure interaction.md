# Describe the tools for interacting with Azure

## Table of contents
* [Azure portal](#azure-portal)
* [Azure Cloud shell](#azure-cloud-shell)
* [Azure powershell](#azure-powershell)
* [Azure CLI](#azure-cli)

## My interpretations

To get the most out of Azure you need a way to interact with its environment, the management groups, subscriptions, resource groups, resources etc.

Azure provides mutiple tools to do so, including:

* Azure portal
* Azure powershell
* Azure CLI (Command line interface)

## Azure portal

Is a web based unified console that provides an alternative to command line tools. With portal you can manage subs, by using GUI.

### Why does it exist?

To provide a unified web based graphical interface (GUI) as a user friendly alternative to using command-line tools.

### What does it do?

It allows you to build, manage and monitor resources, create custom dashboards and configure accessibility settings.

### How does it work?

It is a continuously updated web console hosted in every Azure datacenter to ensure high availability and zero downtime.

### Summary

It is the main website where you log in to manage your cloud services by clicking buttons and viewing visual charts instead of typing code.

## Azure Cloud shell

Is a browser based shell tool that allows you to create, configure and manage Azure resources using a shell. It supports both Azure powershell and Azure CLI which is a Bash shell.

### Why does it exist?

To provide a pre configured, browser-based commandline environment so you can manage Azure without installing tools locally.

### What does it do?

It runs Azure PowerShell and Azure CLI (Bash) scripts directly in your browser while keeping you automatically authenticated.

### How does it work?

You click the terminal icon in the portal: it opens a shell session that inherently knows your identity and permissions.

### Summary

Its a built in command prompt on the Azure website that lets you type commands to manage your cloud instead of clicking buttons.

## Azure powershell

Is a shell with which Devs, DevOps and IT professional can run commands called command lets or cmdlets. These commands call the Azure REST API to perform management tasks in Azure.

### Why does it exist?

To provide a scriptable and repeatable way for professionals to automate Azure management tasks rather than performing them manually in a GUI.

### What does it do?

It runs command lets (cmdlets) that call the Azure REST API to handle routine maintenance, resource setup or massive infrastructure deployments.

### How does it work?

It uses imperative code that can be run as single commands or saved in scripts: it works in Cloud Shell or can be installed on Windows, Linux and Mac.

### Summary

Its a powerful automation tool where you type specific commands to tell Azure exactly what to build or fix, making complex tasks fast and repeatable.

## Azure CLI

Is functionally equal to Azure powershell, with the difference being the syntax of commands. While Azure powershell uses powershell commands, Azure CLI uses bash commands.

### Why does it exist?

To provide a cross platform, command line alternative to the portal that uses Bash style syntax for resource management.

### What does it do?

It performs the same management and automation tasks as powershell, allowing users to script infrastructure changes or handle one off tasks.

### How does it work?

You enter Bash commands into a terminal: it can be used in Cloud Shell or installed locally on Windows, Linux, and macOS.

### Summary

It is a text based control panel for Azure: if you are comfortable with Linux/Bash commands, this is your primary tool for managing the cloud without a mouse.
