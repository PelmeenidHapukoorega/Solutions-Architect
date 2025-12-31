## When to use Bicep

Here I have documented relevant information for myself as to understand when should I use Bicep.

## Is Bicep the right tool?

Main reason for using Bicep is IaC deployments. 

For Azure deployments Bicep has some advantages, but Bicep doesnt work as a language for other cloud providers, which is exactly why I intend to learn Terraform.

## When is Bicep the right tool?

If Azure is the main cloud platform, here are the advantages to consider of using Bicep:

* **Azure-native:** With Bicep, you are using a language that is native to Azure. When new Azure resources are released or updated, Bicep supports those features on day one. When you use other 3rd party tools, it might take some time for new features to be defined in the tool set.
* **Azure integration:** Azure Resource Manager (ARM) templates, both JSON and Bicep are fully integrated within the Azure platform. With ARM deployments, you can monitor the progress of your deployment in the Azure portal.
* **Azure support:** Bicep is a fully supported product with Microsoft Support.
* **No state management:** Bicep deployments compare the current state of your Azure resources with the state that you define in the template. You dont need to keep your resource state information somewhere else, like in a storage account. Azure automatically keeps track of this state for you.
* **Easy transition from JSON:** If you already use JSON templates as your declarative ARM template language, it isnt a difficult process to transition to using Bicep. You can use the Bicep CLI to decompile any ARM template into a Bicep template by using the `bicep decompile` command.

## When is Bicep not the right tool?

Some situations might call for other tool set. Ive listed below reasons not to use Bicep as the main tool set:

* **Existing tool set:** When you are determining when to use Bicep, the first question to ask is, "does my organization already have a tool set in use?" Many tooling options are available that can be used for IaC resource provisioning. Sometimes, it makes sense to use existing financial and knowledge investments when you consider adopting a new process.
* **Multicloud:** If your organization uses multiple cloud providers to host its infrastructure, Bicep might not be the right tool. Other cloud providers dont support Bicep as a template language. Open source tools like Terraform can be used for multicloud deployments, including deployments to Azure.
