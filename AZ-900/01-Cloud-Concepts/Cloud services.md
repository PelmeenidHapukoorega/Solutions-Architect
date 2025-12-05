# Describe Cloud services

## Table of contents
* [Benefits of high availability and scalability](#benefits-of-high-availability-and-scalability)
* [Benefits of reliability and predictability](#benefits-of-reliability-and-predictability)
* [Azure Arc](#azure-arc)
* [Azure VMware](#azure-vmware)
* [Consumption Model](#consumption-model)

## My interprentations

## Benefits of high availability and scalability

## High availability

High availability focuses on ensuring that resources are available regardless of disruptions (meteor strike) or events that may occur.

### WHY does this exist?

It exists so work doesnt stop, business cant afford any downtime nowadays because losing time equals losing money. Its essentially a mechanism that ensures constant business continuity.

### WHAT does it do?

It ensures that business can still continue their work with either minimal to 0 disruptions so everthing on the users end works without any disruptions. Minimal to 0 downtime.

### HOW does it work?

It works by deploying reduntand resources across isolated zones which allows for automating the process of failure detection and switchover. For example a component fails, then it is replaced by a healthy one without causing interruptions on the clients end.

### Summary

High availability allows users to continue their business without minimal to zero downtime which clients wouldnt be able to see or feel.

## Scalability

Scalability means adjusting resources according to the demand.

### Why does this exist?

It exists because it allows users to scale their business in case their systems become overwhelmed by traffic which could result in system failure. Basically if your CPU on your own computer is at 99% constantly it will slow everything down and makes your pc slower. In the cloud it could be felt in a way that website loads a lot longer, throws error and its bad for business and user experience overall. Scalability allows you to allocate more resources so the experience is seamless and your systems work flawlessly with no issues. Its like having a CPU from 10 years ago and suddenly upgrading to new one, everything is faster, smoother and better.

### What does it do?

It allocates resources for your service according to the demand you might be facing. Additionally it allows for more users and more load the more you scale which helps expanding it. 

### How does it work?

It works by allocating resources to meet the demand. Demand increases, you scale, the system becomes more stable. If the user demand drops, you can reduce resources again, which helps with minimizing the costs and only paying for what you use.

### Summary

Scalability allows your systems to stay operational and your clients happy who use your service. Service that works seamlessly is always better than service that lags, throws errors and shuts down because the demand is too high and you failed to scale.

## Vertical scaling

Vertical scaling is the process of increasing or decreasing the capacity of a single resource, like a server or a database.

### Why does this exist?

Because it simplifes the process of adding compute power to an app and to support specific types of workloads that cant be easily distributed across multiple machines. Its simple to implement, in the cloud its as simple as selecting a larger more powerful instance type and just restarting the server. Its not complex.

### What does it do?

It increases the performance and capacity of an application by making its single hosting server more powerful.

### How does it work?

You increase the physical or virtual capacity of a single server instance to improve its performance which in return allows it to handle a larger workload without it buffering.

### Summary

Vertical scaling allows for an easy way to increase workload handling and performance by adding compute power to a single resource which can handle bigger workloads. You can also scale down if needed.

## Horizontal scaling

Horizontal scaling aka scaling out is increasing the capacity of the application by adding more identical servers or computing instances to a distributed pool of resources.

### Why does this exist?

It exists because its the only method that provides virtually limitless capacity, guaranteed high availability and the ability to handle unpredictable traffic demands. It also solves 2 main problems that vertical scaling has which is the ceiling effect and single point favor. In vertical scaling there is a maximum size for a single serves instance. SPOF or single point of failure which means if your one powerful server fails, your entire app goes down, i.e virtual scaling is not fault tolerant, but horizontal scaling is. You can scale horizontally either manually or automatically. Automatically means that systems detects when it needs to scale and adds resources accordingly.

### What does it do?

It enables applications to increase its capacity, maintaing high availability and achieve cost efficiency by utilizing multiple servers working together.

### How does it work?

It distributes the total workload across multiple and identical servers and automatically managing the resource pool to match real time demand by having the load balancer for all incoming requests, auto scaling group that automates the process of adding and removing servers in response to workload changes and stateless architecture. Why stateless? Because the servers need to be disposable and interchangeable ensuring high availability and limitless scalebility.

### Summary

In short horizontal scaling allows for either manual or automatic distribution of compute power according to the workload. Perfect of when you you dont know what your traffic demand is going to look like.

## Benefits of reliability and predictability

## Reliability

Ability of a system to recover from failures and continue to function.

### Why does this exist?

It exists to ensure operational integrity without failure. It means that the services you depend on are always available and perform as expected.

### What does it do?

It delivers its intended function consistently and without unexpected interruptions over time. 

### How does it work?

It works by building a system that can detect, contain and automatically recover from failures across every layer of the technology stack, ensuring that the service remains operational.

### Summary

Reliabilty means that everything works consistently and in case of failure it recovers fast. If your service isnt reliable then there is no real reason why anyone would want to use it if it constantly fails.

## Predictability

Its the ability to consistently deliver a specific level of performance, cost and capacity regardless of changes in the workload, traffic or underlying infra that you built.

### Why does this exist?

Because its essential for businesses to manager risk, control their costs and maintain user trust. All of that combined is needed to succeed.

### What does it do?

Ensure that your app or service behaves consistently and reliably in 3 core areas which are performance, capacity and cost.

### How does it work?

It works by using automation, consistent monitoring and scaling mechanisms to ensure that the app or service delivers expected performance and capacity while keeping your costs stable.

### Summary 

Basically predictability is your way of proving that the system you build is working as intended and consistently. Its like waking up in the morning and knowing you need to go to work i.e predictable behavior.

## Performance predictability

Performance measures overall quality of the service or the app and is a critical factor for user satisfaction and business success.

### Why does this exist?

It exists because its directly tied to user satisfaction, business revenue and operational efficieny. Nobody likes an app or service that is slow, not responsive, has 0 use cases, is not profitable or reliable.

### What does it do?

Ensures that the app or service operate at the optimal speed and efficieny needed to meet user expectations and business goals.

### How does it work?

Performance predictability works by prediciting the resources needed to deliver a positive experience for the customers.

### Summary

Performance predictability ensures your systems are working as intended, your customers are happy, user experience is good, business thrives.

## Cost predictability

Cost predictability is focused on predicting and forecasting the cost of the cloud spend.

### Why does this exist?

It allows you to track your resource use in real time by monitoring them to ensure you are using them efficiently and apply data analytics to find patterns and trends that help better plan your resource deployments.

### What does it do?

Helps you understand better resource deployments, which ones are cost efficient and which ones arent in relation to your business.

### How does it work?

Using cloud analytics and information you can predict your future costs and adjusts resources as needed. You can even use tools like TCO which is Total cost of Ownership or Pricing calculator to get an estimate of potential cloud spend so you can adjust your budget accordingly.

### Summary

Cost predictability allows you to pick resources and adjust them accordingly to your budget without the risk of overspending on resources that you dont need, as well as help predict your future costs.
