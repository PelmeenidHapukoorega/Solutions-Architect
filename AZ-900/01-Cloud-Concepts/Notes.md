# Describe Cloud concepts

## Table of contents
* [What is Shared Responsibility Model?](#what-is-shared-responsibility-model)
* [Cloud Models](#cloud-models)
* [Azure Arc](#azure-arc)
* [Azure VMware](#azure-vmware)
* [Consumption Model](#consumption-model)

## What is cloud computing?

* Cloud computing is the delivery of computing services (like virtual machines, storage, databases, and networking) over the internet.
* Cloud services also include advanced technology offerings such as Internet of Things (IoT), machine learning (ML), and artificial intelligence (AI).
* Because it uses the internet, it is not constrained by physical infrastructure like a traditional datacenter.
* This flexibility allows you to rapidly expand your IT infrastructure without waiting to build new physical facilities.


### My interpretation

### WHY does this exist?
* What pain points did the businesses have?
* What limitations existed before?
* What is the purpose behind its creation?

Cloud computing was made with business in mind in a sense that before you had to have your own physical infra, workforce,
spending a lot of money for equipment that would handle storage, data etc. Cloud tackled that problem by letting users do all of that in the cloud,
with only spending money on resources that you as the user actually use. Before cloud the main limitations was needing a lot of money to build datacentres, 
hire additional personnel to manage each sector of different computing aspects, cloud lets you do it all in one place.

### WHAT does it do?
* What is its job?
* What role does it play?
* What solution does it provide?

Cloud computings job is to do everthing that is usually done by having physical infra,
but over the internet and in a more simplified and efficient way. You dont need to set up physical server, then additionally
install VM on it, the configure network for it seperately. It lets you do all that in a whim and almost instantly in one platform.
You dont need to buy seperate equipment for say storage, or seperate equipment for handling network. 
It lets you do everything in one place and thats the magic.

### HOW does it work? 
* How does the service behave?
* The steps or flow
* What happens behind the scenes?

The service works over the internet, for example you log into the portal, you create a resource like VM, 
you set the configs, the rules you need and then it spins it up. 
In the background that information goes to clouds own datacentre over the internet and creates what you set up.

### Summary

Bsically Cloud computing is as having physical infra and all the workers but instead on the internet and without the workers.
It helps businesses keep everything in 1 place without the need for various equipment, its cost efficient and easier to manager than phsysical infra.

## What is shared responsibility model?

* In a traditional corporate datacenter, the company (consumer) is responsible for everything, including the physical space, servers, networking, software, patching, and security.
* With cloud computing, these responsibilities are shared, meaning the burden is significantly reduced for the consumer.

## My interprentation

### WHY does this exist?

The shared responsibility model was created to set clear boundaries as in who is responsible for what exactly to avoid confusion. Cloud provider for example is responsible for the physical side of things, such as network, datacentre and the security of it. The user or consumer is responsible for how they do things within the cloud. Otherwise people would assume that they move all their infra and stuff to the cloud and if anything breaks the cloud service provider would be responsible for everthing, including the mistakes the consumers themselves did. 
Responsibility also depends on the service type, if its IaaS its mostly on the user because they create most of it. In SaaS its mostly on cloud provider because they offer the applications and tools necessary for the creation, therefore they are responsible for how they operate, In PaaS its the middle ground of both because you create the platform but the cloud provider offers you tool for the creation of it. Basically responsibility model was created to clearly distinguish who is responsible for what if stuff breaks.

[IaaS vs SaaS vs PaaS](./Diagrams/IaaS%20vs%20SaaS%20vs%20PaaS.md)

### WHAT does it do?

Shared responsibility sets clear boundaries who is responsible for what. It forces customers to actually think for a second before making an informed decision about what service type they will use. Do they want more responsibility or less? if they need IaaS service type then they will need to be comfortable with the idea of being responsible for most things in the cloud.
It solves the confusion as in everbody knows what they are responsible and to take accountability if stuff breaks, in addition since its clearly defined its easier to distinguish who to turn to if in need of help with a service regarding a specific issue.

### HOW does it work? 

Shared responsibility model works in a way that it sorts everthing, as to what to do, what to pick, who to turn to.
If you pick a service type then you have a clear direction and you know exactly who is responsible for what.
The provider focuses on the security and physical side of things, and the user is responsible for what they put in the cloud and security within the cloud. The seperation ensures that different tasks are handled by the party best equipped to do so.

[Shared Responsibility Model Diagram](./Diagrams/Shared%20responsibilty%20model.md)

### Summary

Basically shared responsibility model clearly seperates the responsibility between partys and forces accounatibility accordingly.

## Cloud models

Cloud models define the deployment type of cloud resources. As in if its private then you own the cloud. If its public its provided by someone. If its hybrid its like having both.

[Cloud Models](./Diagrams/Cloud%20Models.md)

### WHY does this exist?

Different cloud models exist because there are different needs to be met, you cant have one cloud for everthing because you cant simply please everyone.

### WHAT does it do?

By having different cloud models it offers diversity. Some like to outsource the service because they dont have the money to host their own datacentre. Others like to have their own cloud, because they have the resources for it and they get to decide over how everything works. Then there the ones who might have say the phsyicial infrastructure for it, but dont have other tools needed for their work, so they use cloud services for some of the things.

### HOW does it work? 

It all works somewhat the same way, the key difference is who controls what and where do the resources live.

## Private cloud

Private cloud is basically a corpo that decided to move all their resources into cloud by creating their own instead of outsourcing a provider.

### WHY does this exist?

It exists because if you have private cloud, you control and own everything, the downside being that you are also responsible for everything. Say the network goes down then you have to fix it, you cant call up Azure helpdesk for example and tell them to solve your mess. You could also host your private cloud by 3rd party but they need to dedicate a datacentre especially for you which means that you are paying.

### WHAT does it do?

It gives you full control over the company itself and its IT department. You decide everything, but you are also responsible for everything. Its also not that cost friendly, because you if you need to update your hardware you need to buy it and configure it from 0 for it to work.

### HOW does it work? 

It maximises control over your resources, you are not reliant on anyone. If you use a cloud provider and say a region gets hit with a meteorite then you cant do anything about it. But if you own it, you could build the datacentre into a bunker if you decide so.

### Summary

Private cloud is something you own, you decide, you are responsible and you control everything.

## Public cloud

Public cloud is built and maintained by the service provider who owns it, meaning you are not responsible for the physical side of things. Anyone who wants to move to cloud can do so, its available to anyone including individual people who say want to build infra or apps but dont have the money or resources to build their own. It opens up the cloud world to anyone who wants to use it for their own stuff.

### WHAT does it do?

It provides near infinite scalebility, its extremely cost efficient since you pay for what you use. Physical side of things is managed by the provider.

### WHY does this exist?

It exists because it gives users more freedom to actually use cloud without having to worry about money, equipment, network connectivity etc. Its publicly available to anyone, shares the burden, its cost efficient and extremely easy to scale and become bigger.

### HOW does it work? 

It works by having massive infrastructure, a lot of resources to be used and it can be offered to multiple customers at once and over the public internet. Its also vastly automated, meaning you dont have to perform complext tasks yourself to set something up.

### Summary

Basically public cloud is like having a huge datacentre that you can use for whatever over the internet, you dont need to have your own equipment, your own datacentre, you dont spend vasts amount, complex tasks are automated, shared responsibility, less stress and more freedom and you only pay for what you actually use.

## Hybrid cloud

Hybrid cloud is like having both public and private cloud. You can choose where you keep what resources, you can also seperate things easily. For example you could use PaaS but keep resources in public cloud because then the vast majority of the responsibility is on the cloud provider and you pay for what you use, so its also cost efficient. Everything else that you consider sensitive you can keep in your own cloud. In addition if you want to scale fast at some point you could move your resources to public cloud since it uses secure connection. So basically its the best of both. 

### WHAT does it do?

Hybrid cloud allows you to pick and choose where and how you manage your data based on your specific needs.

### WHY does this exist?

Because it solves a problem neither Private or Public cloud alone can solve. Say you work in private sector where information is on a need to know basis, you wouldnt hold sensitive information in the public cloud but you would hold it in private cloud, but having private cloud alone is costly to say the least. So what do you do? You use both, store sensitive info in your own cloud and if you want to scale or use other or use more public friendly services but dont want to spend more? You can move that into public cloud at will.

### HOW does it work? 

It works by establishing a secure connection between both Private and Public cloud, you can store data as needed without having the need to compromise. Say you have physical infra and you want to migrate to cloud, but migration doesnt happen in an instant, it takes time and you obviously would want to pick and choose what you migrate and what you want to have on premises, thats where hybrid cloud comes into play.

### Summary

Hybrid cloud is best of both worlds because you can pick and choose what you keep where, it gives you control over your data and where you store it. It also allows you to move applications or data that you dont consider sensitive but it is costly to manage on prem to be stored and used in public cloud, because there you pay for what you use.

## Multi cloud

Multi cloud is basically using everything, it allows for complete freedom over everthing, security over sensitive data, ability to scale near infinitely. Perhaps you like features of different cloud providers and want to use them. Perhaps you want to keep certain stuff in one cloud provider over the other because its easier to manage in one than the other, or its cheaper than in the other etc etc.

### WHAT does it do?

Multi cloud allows for resource usage optimization, managing risk and its not reliant on a singular provider, its a mixture of everything, but its also hassle to manage as far as i can see, unless you use CMPs which is a unified dashboard for all cloud providers for managing resources and data.

### WHY does this exist?

Multi cloud exists because everyone has different needs and wants, its managing risks, and it helps you customise your infrastructure on all sides. Wanna store sensitive data? Use on prem. Want near infinite scalability and cost efficiency? Use public cloud. Dont like certain services offered by one cloud provider? Fine, use the other one.

### HOW does it work? 

Multi cloud works by having different types of cloud modules depending on what you want to accomplish and what your goal with your resources, data and business needs are. In my eyes it allows for total freedom and customisation over how you want to do things. You can handpick everything.

### Summary

Multi cloud is perfect if you have a lot of different needs and concerns about your business needs. You get to handpick where you keep what, what services you use from what cloud provider and put into a mixed bag. Total customisation.

## Azure Arc

Azure arc is a set of technologies that helps manage your cloud environment.

### WHAT does it do?

It helps you manage your cloud whether its private, public, hybrid or multi cloud that runs on multiple cloud providers at once. Additionally it extents Azure control plane which is the central management layer which allows you to set rules, manage servers, clusters and data services located anywhere.

### WHY does this exist?

It solves the modern challenge of managing IT resources that are spread out everywhere, in short its purpose is to make anything look and act like its a native resource of Azure. Think unified control panel for management.

### HOW does it work? 

It works by installing a small piece of software on a resource that is outside Azure cloud but it tricks the Azure system into thinking that resources that it is applied to are hosted on Azure. 
You install the Azure arc agent onto the machine or cluster living in either datacentre, aws or gcp. The agent establishes secure outbound connection to Azure management service. Then the external resource is registered and appears as standard resource withing Azure portal and recieves a unique Azure resource ID.

### Summary

Azure arc acts like a translator and connector that brings both management, governance and deployment capabilities of Azure to your entire IT infra regardless of location.

## Azure VMware

Say you have a lot of VMs on VMware that are organized in your way, but now you want to move to a bigger playground which is Azure. Instead of taking them all out and re organize everthing for Azure, Azure VMware Solution or AVS lifts everything at once and places it right into Azure.

### WHAT does it do?

It creates software that enables one physical server to run multiple isolate VM systems at the same time

### WHY does this exist?

It exists because it allows large companies to move ther existing VMware systems quickly into cloud without having to rebuild them brick by brick, all while retaining their familiar management tools.

### HOW does it work? 

It works by hosting customers dedicated VMware environment on the physical serves within Azure data centre, allowing seamless migration and integration to Azure services.

## Consumption model

Consumption based model is literally pay as you go. Meaning that it has no upfront costs, you dont need to purchase costly infrastructure that you might not even use to its full potential, you pay only for resources that you actually and you can stop paying for resources that you no longer need either currently or once you expand your service. You can gradually check and expand and delete based on your current needs. 

### WHAT does it do?

It allows consumers to pick and choose how to scale, what resources to use, avoid upfront costs, delete resources that are no longer neede. No need for physical infra itself, no need for workforce for maintenance and additional equipment for power, cooling etc

### WHY does this exist?

It exists because its cheaper and allows businesses to still do their work with less management, less costs and more freedom and efficiency.

### HOW does it work? 

It meters and tracks users resources that are actually being used and then later being billed for them, so you dont have to pay anything up front. You set up what you need to set up, for example spin up couple VMs, storage blobs and so on and then later you are being billed for their usage.

### Summary

Consumption model or pay as you go model allows consumers to set up their service types and infra by avoiding the need for physical infra and additonal equipment by letting users pay only for the resources they actually use.


