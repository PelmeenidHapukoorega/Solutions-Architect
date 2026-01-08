# Bicep Module Architecture

## Why Modules?
**Purpose:** Address "Monolithic Code" issues (hard to read, duplicate logic) by splitting a single template into smaller, independent files.

### Key Benefits
1.  **Reusability:** A module built for one project (e.g., `networking.bicep`) can be referenced by multiple other projects without copy-pasting code.
2.  **Encapsulation:** Groups related resources (e.g., Function App + Storage + Hosting Plan) into a single logical unit. The parent template doesn't need to know *how* the Function App is built, only that it *is* built.
3.  **Composability:** Allows chaining inputs/outputs. The Output of Module A (e.g., VNET ID) becomes the Parameter for Module B (e.g., VM).

## Implementation Strategy

### Identifying Module Candidates
**What to group:**
*   **Good:** Related sets of resources (Database Server + Database + Firewall Rules).
*   **Bad:** Creating a separate module for every single resource (Over-engineering).

**Tooling:** Use the **Bicep Visualizer** in VS Code (Right-click file -> Open Bicep Visualizer) to see clusters of dependencies. These clusters are prime candidates for modularization.

### Syntax for Usage
The `module` keyword consumes another Bicep file.

```bicep
module appModule 'modules/app.bicep' = {
  name: 'myAppDeployment' // Unique Deployment Name
  params: {
    location: location
    appServiceAppName: appServiceAppName
  }
}
```

**Components**

1. **Symbolic Name (`appModule`):** Internal reference within the parent file.
2. **Path (`'modules/app.bicep'`):** Relative path to the file.
3. **Name Property:** The name of the Deployment resource Azure creates (see below).
4. **Params:** Input values required by the module.

## Deployment mechanics

**The `Microsoft.Resources/deployments` Resource**

Every time you deploy a Bicep file, Azure creates a Deployment Resource to track the operation.

* **Without Modules:** 1 Deployment Resource (named after the file, e.g., main).
* **With Modules:** Nested Deployment Resources.
  * **Parent Deployment:** `main`.
  * **Module Deployment:** `myAppDeployment` (Created by the parent).

**Transpilation**

Bicep compiles all referenced modules into a single JSON ARM Template before sending it to Azure.

* **Input:** `main.bicep` + `module1.bicep` + `module2.bicep`.
* **Output:** `main.json` (One monolithic file containing everything).

**Naming collisions**

Deployment history is stored by name. If you reuse the name `myAppDeployment`, the history of the previous run with that name is overwritten. To keep history, use unique deployment names (e.g., append a timestamp).
