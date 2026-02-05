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

### What i learned

### Key Takeaways

