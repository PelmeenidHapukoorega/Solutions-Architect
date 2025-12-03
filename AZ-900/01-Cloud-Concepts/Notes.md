# Describe Cloud concepts

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
* What pain points did the businesses have?
* What limitations existed before?
* What is the purpose behind its creation?

The shared responsibility model was created to set clear boundaries as in who is responsible for what exactly to avoid confusion. Cloud provider for example is responsible for the physical side of things, such as network, datacentre and the security of it. The user or consumer is responsible for how they do things within the cloud. Otherwise people would assume that they move all their infra and stuff to the cloud and if anything breaks the cloud service provider would be responsible for everthing, including the mistakes the consumers themselves did. 
Responsibility also depends on the service type, if its IaaS its mostly on the user because they create most of it. In SaaS its mostly on cloud provider because they offer the applications and tools necessary for the creation, therefore they are responsible for how they operate, In PaaS its the middle ground of both because you create the platform but the cloud provider offers you tool for the creation of it. Basically responsibility model was created to clearly distinguish who is responsible for what if stuff breaks.

### WHAT does it do?
* What is its job?
* What role does it play?
* What solution does it provide?

Shared responsibility sets clear boundaries who is responsible for what. It forces customers to actually think for a second before making an informed decision about what service type they will use. Do they want more responsibility or less? if they need IaaS service type then they will need to be comfortable with the idea of being responsible for most things in the cloud.
It solves the confusion as in everbody knows what they are responsible and to take accountability if stuff breaks, in addition since its clearly defined its easier to distinguish who to turn to if in need of help with a service regarding a specific issue.

### HOW does it work? 
* How does the service behave?
* The steps or flow
* What happens behind the scenes?

Shared responsibility model works in a way that it sorts everthing, as to what to do, what to pick, who to turn to.
If you pick a service type then you have a clear direction and you know exactly who is responsible for what.
The provider focuses on the security and physical side of things, and the user is responsible for what they put in the cloud and security within the cloud. The seperation ensures that different tasks are handled by the party best equipped to do so.

[Shared Responsibility Model Diagram](./Diagrams/Shared%20responsibilty%20model.md)

### Summary

Basically shared responsibility model clearly seperates the responsibility between partys and forces accounatibility accordingly.






