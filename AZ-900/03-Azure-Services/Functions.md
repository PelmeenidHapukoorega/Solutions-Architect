# Describe Azure functions

## My interpretations

Azure functions is an event driven, serverless compute option that requires no maintaining of VMs or containers. Whenever you create resources they enter status "running" in order for the app to function. With functions an event wakes the function which alleviates the need to keep resources provisioned when nothing is going on.

### Why does it exist?

To allow you to build event driven apps faster and cheaper by elminating the need to manager servers or infra.

### What does it do?

It runs small pieces of code that you as the user put in within the cloud in response to specific event without the need to manage any servers.

### How does it work?

You create a trigger to your code which starts the execution, when the trigger occurs the azure functions takes over. Functions then uses bindings to simplify how your code connects to other services, it pulls it from the another service and makes it available to your function code as a simple input variable. Then it automatically takes the result from your function and writes in another service without you needing to write boilerplate code for external SDKs.

### Benefits of Azure functions

* Ideal when concerned about the code running your service.
* Commonly used to perform work in response to an event, timer or message from another Azure service.
* Can complete the work within seconds or less.
* Scale automaticall based on demand, good choice when demand is variable.
* Only charges you for the CPU time used when the function runs.

**Stateless**
  * Behaving as if it restarts every time it responds to an event. Functions are stateless by default.
**Stateful**
  * Context is passed through the function to track prior activity

Functions are also general compute platform for running any type of code. If the needs change you can deploy the project in an environment that isnt serverless. This allows you to manage scaling, run on virtual networks and even completely isolate the functions.

### Summary

Functions allows you to automate and build event driven apps cheaper without the need to manage servers or the infra. Since its responding to events means its automated which in return allows you to focus on the code.
