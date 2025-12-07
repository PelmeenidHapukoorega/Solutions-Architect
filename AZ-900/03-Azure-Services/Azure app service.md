# Describe Azure app service

## Table of contents
* [Types of app services](#types-of-app-services)
* [Web apps](#web-apps)
* [API apps](#api-apps)
* [Web jobs](#web-jobs)
* [Mobile apps](#mobile-apps)

## My interpretations

App service enables you to build and host web apps, baground jobs, mobile back ends and RESTful APIs in the programming language of your choice without managing infrastructure. Additionally it offers automatic scaling and high availability. App service supports both windows and linux, enables automated deployments for Github, azure devops or any git repo to support a continuous deployment model.

### Why does it exist?

App service exists as PaaS offering developers to build, deploy and scale web apps and APIs without the management of infra.

### What does it do?

Provides easy to use environment for hosting web applications, APIs and mobile backends by automatically handling infrastructure, maintenance and dployment complexities.

### How does it work?

It works by utilizing an invisible layer of dedicated Azure infra to host and manage your code, and moving away from underlying servers and OS systems.

### Types of app services

With app service you can host the following common app service styles:

* Web apps
* API apps
* WebJobs
* Mobile apps

App service handles most of the infra decisions you would deal with in hosting web accessible apps like:

* Deployment and management, which are integrated to the platform
* Endpoints can be secured
* Sites can be scaled quickly for incoming high traffic loads
* Built in load balancer which automatically allocates resources where its needed to provide high availability based on defined rules.

## Web apps

App service includes full support for hosting web apps by using ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP or Python. You can choose either windows or linux for OS.

### Why does it exist?

Because web apps solve problems associated with traditional desktop applications, specifically the need for cross platform compatibility, centralized data and simplified deployment and updates.

### What does it do?

Provides users with access to software and services through a standard web browser by eliminating the need for local installation, centralizing your data and enabling real time collaboration between users.

### How does it work?

Web apps work through a continuous client/server interaction, where the users web browser aka client sends requests over the internet to a remote server, which then processes the request and sends back necessary content.

### Summary

Web app is like a vending machine for software. You walk up to it, pick what you want from the machine (can of coca cola) and then wait til the vending machnine processes your number input before it drops the coca cola at the bottom.

## API apps

API apps are fully managed PaaS feature within the app service, desigend specifically for easily developing, hosting and consuming RESTful web APIs. Its like a specialized version of Azure web apps specifically optimized for the tasks APIs need, rather than hosting a user facing website.

### Why does it exist?

Because it provides developers with a fully managed, feature rich PaaS environment that is optimized for hosting, securing and scaling, without the need to manage underlying web servers or infra.

### What does it do?

It hosts, secures and scales RESTful APIs.

### How does it work?

Works like Azure web apps, its a process of continous loop of requests and responses focused on data transfer. Users web app, mobile app or another server sends HTTP reques to a specified endpoint hosted by the API app. This request contains either data or asks the API to perform an action. The underlying infra recieves the request, your deployed API code runs, performs necessary actions. Then the API code packages the result of the processings, sends back HTTP response typically in either JSON or XML. If request was successful the server sends success code.

### Summary

API apps are similar to web apps. Web app is like a storefront, and API is the warehouse manager. API apps job is to rpocess requests for data or execute commands and return the result in a machine readable format aka JSON or XML. I.e web app is what you see and interact with, API app is the powerful engine working behind the scenes that the web app talks to. Its like the system behind the vending machine.

## Web jobs

Web jobs is a built in feature of app service that allows you to run background tasks, scripts and programs alongside your main web application.

### Why does it exist?

To provide user with background tasks, scripts and long running processes easily and cost effectively, using the same infra as the app service (Web app or API app)

### What does it do?

Runs background tasks, scripts and programs alongside your main web application or API.

### How does it work?

It executes code as background process on the same set of VMs that host your maint app service (web app or API app) utilizing simple built in management system and a runtime monitor. You deploy the script or executable program directly to existing app service.

### Summry

In short web jobs is the process manager on your existing service infra that performs scheduled or continous tasks. Basically its like the maintenance guy for the vending machine.

## Mobile apps

Mobile apps feature of app service allows you to quickly build a back end for iOS and android apps. With just a few clicks in the portal you can:

* Store mobile data in a cloud based SQL database.
* Authenticate customers against common social providers such as MSA, google X and facebook.
* Send push notifications.
* Execute custom back end logic in C# or node.js.

On the mobile app side there is SDK support for native iOS and android, xamarin and react native apps.
### Summary

Basically you can think of app service as a sort of container. Its a fully managed service that uses containers as the basic building block to run your applications, which gives you all the benefits of containers, just without the work of managing them.
