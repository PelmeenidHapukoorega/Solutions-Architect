# Lab 9c: Implementing container apps

### Goal:

To learn how to implement and deploy container apps.

```ascii
+--------------------------------+       +--------------------------------+
| TASK 1                         |       | TASK 2                         |
| [ az104-rg9 ]                  |       |                                |
|                                |       |  +--------------------------+  |
|  +--------------------------+  |       |  |                          |  |
|  | Container Apps           |  |       |  |     Test and verify      |  |
|  | Environment              |  |       |  |            |             |  |
|  |                          |  |       |  +------------|-------------+  |
|  |                          |  |       |               |                |
|  | Container App            | <--------|---------------┘                |
|  |                          |  |       |                                |
|  +--------------------------+  |       |                                |
+--------------------------------+       +--------------------------------+
```

**Job Skills**

* Creating and configuring container apps and environments.
* Testing and verifying deployment of container apps.

## Part 1: Creating and configuring container app and environment

1. In the portal went to `container apps` and created new one.
2. In the `basics` tab added resource group and following values:

* RG `az104-rg9`
* Named container app to `my-app`
* Region > norway east
* Container apps environment > selected `create new` > set the environment name to `my-environment` > `create`

3. In the `container` tab ensured that `use quickstart image` was checked and made sure that `quickstart image` was set to simple `hello world` container.

Hit review and then hit create:

<img width="636" height="737" alt="image" src="https://github.com/user-attachments/assets/c5b2947f-36ad-4fc6-8c15-a5e88a8b6314" />

## Part 2: tested and verified deployment of the container app

1. After the app was deployed i went to the resource.
2. Next to `application URL` i copied the URL and pasted it into browser:

<img width="778" height="809" alt="image" src="https://github.com/user-attachments/assets/ec1ab321-eae5-4054-81ce-7b049f5f75af" />

Performed cleanup

### Summary

Started by creating a new container app. Added it to my existing resource group, named it `my‑app`, selected norway east as region, and created a new container apps environment called `my‑environment`. Used the quickstart “hello world” container image to keep the deployment simple.

Deployed the app and then verified it by opening the application URL in the browser. The page loaded successfully, confirming that the container app was running inside the environment as expected.

Performed cleanup once the test was complete.

### What i learned

* Learned how container apps differ from container instances by giving you an environment layer for running multiple apps or microservices.
* Learned how quickstart images make it easy to deploy a working container without building anything locally.
* Learned how simple it is to verify a container app through its public endpoint.
* Learned that container apps provide a managed, serverless style experience for container workloads.

### Key Takeaways

* Azure Container Apps (ACA) is a serverless platform that allows you to maintain less infrastructure and save costs while running containerized applications.
* Container Apps provides server configuration, container orchestration, and deployment details.
* Workloads on ACA are usually long-running processes like a Web App.
