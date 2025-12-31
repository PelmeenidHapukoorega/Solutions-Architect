## How Bicep works

Here I explain how Bicep works on paper before actually getting into deployment of templates using Bicep.

## Bicep deployment

When you deploy a resource or series of them to Azure, you submit the Bicep template to ARM which still requires JSON templates. 

The tooling built into Bicep converts your Bicep template into JSON template. This process is known as "transpilation", which in essence treats the ARM template as the intermediate language.

The conversion happens automatically when you submit your deployment or you can do it manually.

See here:

<img width="418" height="265" alt="image" src="https://github.com/user-attachments/assets/26d80a5b-d403-4984-8ee6-d3e4deb39ce2" />

**Note!**

Transpilation is the process of converting source code written in 1 language into another language.

Latest version of Azure CLI and PWSL have built in Bicep support. Which means you can use the same deployment commands to deploy Bicep and JSON templates.

For example, the following command deploys a Bicep template to a resource group named `storage-resource-group`:

```CLI
az deployment group create \
--template-file main.bicep \
--resource-group storage-resource-group
```

When this is submitted ARM will look at the resources currently deployed in Azure.

Then it looks at what you are trying to deploy, and sets up a sequence of steps to achieve this state. All these steps invole invoking ARM API.

You can view JSON template submitted to ARM by using `bicep build` command.

The following demonstrates how Bicep template is converted into its corresponding JSON template:

```bash
bicep build main.bicep
```

## Comparing JSON and Bicep

Bicep provides a simpler syntax to use when you're writing templates. The template on the left side of the screen is a Bicep template. The template on the right side of the screen is a JSON template.

<img width="1526" height="796" alt="image" src="https://github.com/user-attachments/assets/ffb9fda5-0d86-4cf1-88ac-49e19f40bf25" />

From this I can see the difference immediately just by the amount of code required to function. Syntax is easier to follow and comprehend and there are no complex expressions like in the JSON template shown on the right.

**Note!**

To view equivalent JSON and Bicep files side by side see Bicep Playground: https://azure.github.io/bicep/
