# Describe Azure Resource Manager and its templates.

## Table of contents
* [Azure Resource Manager](#azure-resource-manager)
* [Infra as code](#infra-as-code)
* [ARM templates](#arm-templates)
* [Bicep](#bicep)

## My interpretations

## Azure Resource Manager

Is the deployment and management service of Azure. It provides management layer that enables you to create, update and delete resources in your account.

### Why does it exist?

To provide a single unified management layer that ensures every action in Azure is consistent, secure and authorized.

### What does it do?

It acts as the central gateway for creating, updating and deleting resources, handling authentication and routing requests to the right services.

### How does it work?

It intercepts requests from all tools (Portal, CLI, PowerShell etc) via its API, verifies permissions and then instructs the specific Azure service to act.

### Summary

ARM is the Middleman or Brain of Azure: every time you click a button or run a command, ARM is the one that actually processes the order.

Benefits of ARM:

* Manage your infrastructure through declarative templates rather than scripts. A Resource Manager template is a JSON file that defines what you want to deploy to Azure.
* Deploy, manage, and monitor all the resources for your solution as a group, rather than handling these resources individually.
* Re deploy your solution throughout the development life-cycle and have confidence your resources are deployed in a consistent state.
* Define the dependencies between resources, so they are deployed in the correct order.
* Apply access control to all services because RBAC is natively integrated into the management platform.
* Apply tags to resources to logically organize all the resources in your subscription.
* Clarify your organizations billing by viewing costs for a group of resources that share the same tag.

## Infra as code

Is a concept where you manage your infrastructure as lines of code. At the basic level its things like using Azure cloud shell, powershell or the CLI to manage and configure resources.

### Why does it exist?

To replace manual error prone configurations with repeatable automated processes that treat infrastructure like software.

### What does it do?

It manages and provisions entire environments from simple apps to complex networks using lines of code instead of manual clicks.

### How does it work?

It uses scripts (PowerShell/CLI) or templates (ARM/Bicep) to send a set of instructions to Azure Resource Manager to build resources.

### Summary

Its like having a digital blueprint or recipe, instead of building a house by hand every time, you run a program that builds it perfectly for you.

## ARM templates

Using ARM templates you can describe resources you want to use in a declarative JSON format. With ARM template the deployment code is verified before any code is run. This ensures that resources created will be created and connected correctly.

### Why does it exist?

To provide a declarative way to define infrastructure ensuring environments are consistent, predictable and fast to deploy.

### What does it do?

It allows you to define the desired state of your resources in a file, the system then handles the HOW of building and connecting them.

### How does it work?

Uses JSON files that are verified before execution, it then orchestrates deployments in parallel (creating multiple items simultaneously).

### Summary

Its a smart shopping list for cloud: you list what you want, and Azure builds it all at once, perfectly, every single time.

### Benefits of ARM templates

* Declarative syntax: ARM templates allow you to create and deploy an entire Azure infrastructure declaratively. Declarative syntax means you declare what you want to deploy but dont need to write the actual programming commands and sequence to deploy the resources.
* Repeatable results: Repeatedly deploy your infra throughout the development lifecycle and have confidence your resources are deployed in a consistent manner. You can use the same ARM template to deploy multiple dev/test environments knowing that all the environments are the same.
* Orchestration: You dont have to worry about the complexities of ordering operations. Azure Resource Manager orchestrates the deployment of interdependent resources, so they are created in the correct order. When possible ARM deploys resources in parallel, so your deployments finish faster than serial deployments. You deploy the template through one command rather than through multiple imperative commands.
* Modular files: You can break your templates into smaller reusable components and link them together at deployment time. You can also nest one template inside another template. For example you could create a template for a VM stack, and then nest that template inside of templates that deploy entire environments, and that VM stack will consistently be deployed in each of the environment templates.
* Extensibility: With deployment scripts you can add powershell or bash scripts to your templates. The deployment scripts extend your ability to set up resources during deployment. A script can be included in the template or stored in an external source and referenced in the template. Deployment scripts give you the ability to complete your E2E environment setup in a single ARM template.

## Bicep

Is a language that uses declaritive syntax to deploy Azure resources. Bicep file defines infra and configurations before ARM deploys that environment based on your Bicep file. Bicep uses fairly simple concise style.

### Why does it exist?

To provide a more readable and concise way to write infra as code compared to the complex JSON used in ARM templates.

### What does it do?

It uses declarative syntax to define exactly what your Azure environment should look like, then hands that plan to ARM for deployment.

### How does it work?

You write a .bicep file (for example in VSC) Azure processes it (often converting it to JSON internally) and uses the Azure Resource Manager to build the resources.

### Summary

Its the clean handwriting version of an ARM template. It does the same job but is much easier to read and write.

### Benefits of Bicep

* Support for all resource types and API versions: Bicep immediately supports all preview and GA versions for Azure services. As soon as a resource provider introduces new resource types and API versions, you can use them in your Bicep file. You dont have to wait for tools to be updated before using the new services.

* Simple syntax: When compared to the equivalent JSON template, Bicep files are more concise and easier to read. Bicep requires no previous knowledge of programming languages. Bicep syntax is declarative and specifies which resources and resource properties you want to deploy.

* Repeatable results: Repeatedly deploy your infrastructure throughout the development lifecycle and have confidence your resources are deployed in a consistent manner. Bicep files are idempotent, which means you can deploy the same file many times and get the same resource types in the same state. You can develop one file that represents the desired state, rather than developing lots of separate files to represent updates.

* Orchestration: You dont have to worry about the complexities of ordering operations. ARM orchestrates the deployment of interdependent resources so they are created in the correct order. When possible ARM deploys resources in parallel so your deployments finish faster than serial deployments. You deploy the file through one command, rather than through multiple imperative commands.

* Modularity: You can break your Bicep code into manageable parts by using modules. The module deploys a set of related resources. Modules enable you to reuse code and simplify development. Add the module to a Bicep file anytime you need to deploy those resources.
