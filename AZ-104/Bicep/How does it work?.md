## How Bicep works

Here I explain how Bicep works on paper before actually getting into deployment of templates using Bicep.

## Bicep deployment

When you deploy a resource or series of them to Azure, you submit the Bicep template to ARM which still requires JSON templates. 

The tooling built into Bicep converts your Bicep template into JSON template. This process is known as "transpilation", which in essence treats
