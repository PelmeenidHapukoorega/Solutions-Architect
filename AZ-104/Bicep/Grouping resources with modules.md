# Grouping Resources with Bicep Modules

## 1. The Concept: Modularization
**Goal:** Manage complexity and increase code reusability by breaking large Bicep files into smaller, manageable components called **Modules**.

*   **How it works:** A "Main" Bicep file references other Bicep files (modules).
*   **Transpilation:** Behind the scenes, Bicep combines the main file and all modules into a single JSON ARM template for deployment.
*   **Reusability:** A module created for one solution (e.g., a Networking module) can be reused in completely different solutions.

## 2. Bicep Outputs
**Purpose:** Outputs allow a Bicep file (or module) to send data back to the user or pipeline after deployment.

### Syntax
```bicep
output appServiceAppName string = appServiceAppName
```

* **Keyword:** `output` defines the return value.
* **Name:** `appServiceAppName` (This is how you access the value later).
* **Type:** `string` (Supports same types as parameters).
* **Value:** Must always have a value assigned (can be a variable, parameter, or resource property).

### Use cases

1. Returning a Public IP address to SSH into a VM.
2. Returning a generated Resource Name to use in a CI/CD pipeline.


### Best practices

* **Do:** Use dynamic resource properties instead of hardcoded assumptions.
  * Good: `output url string = app.properties.defaultHostName`
  * Bad: `output url string = 'https://my-hardcoded-name.azurewebsites.net'`
* **Dont:** **NEVER** output secrets (Connection Strings, API Keys, Passwords). Outputs are visible in clear text to anyone with read access to the Resource Group.

## Defining a module

**Definition:** Any standard Bicep file can be used as a module by another Bicep file.

### Syntax

To import a module into a parent file, use the module keyword:

```Bicep
module myModule 'modules/mymodule.bicep' = {
  name: 'MyModuleDeployment'
  params: {
    location: location
  }
}
```

### Key components

* **Symbolic Name (`myModule`):** Used to reference the module within the parent file (e.g., to access its outputs).
* **Path (`'modules/...'`):** Relative path to the Bicep file being used as a module.
* **Name Property (`name`):** **Mandatory**. Azure creates a separate nested deployment for each module; this sets the name of that deployment in the Azure logs.
* **Params (`params`):** Passes values from the parent file into the modules parameters.

## Module design principles

* **Logical Grouping:** Modules should have a clear purpose (e.g., "Networking Stack" or "Database Layer").
* **Avoid Over-Granularity:** Do not create a separate module for every single resource. Modules should generally combine related resources.
* **Self-Contained:** If a module relies on a variable, define that variable inside the module, not the parent.
* **Chaining:** You can take the Output of Module A and pass it as a Parameter into Module B.
