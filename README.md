# Azure Template for Removing Elevated Access
## Overview

This repository contains an Azure Resource Manager (ARM) template and a parameters file to deploy resources for removing elevated access (User Access Administrator role) from a user using an Azure Automation Account with Managed Identity. The setup includes a Logic App to automate the process.

For a detailed step-by-step guide, please refer to the blog post on the solution: https://www.chanceofsecurity.com/post/restrict-elevated-access-microsoft-entra-logic-app

## Contents

- `template.json`: The ARM template defining the resources to be deployed.
- `parameters.json`: The parameters file to customize the deployment.
- `README.md`: This documentation file.

## Prerequisites

Before deploying the template, ensure you have the following:

- An Azure subscription.
- Azure CLI or Azure PowerShell installed.
- Appropriate permissions to create resources in the target subscription and resource group.

## Deployment

### Step 1: Clone the Repository

Clone this repository to your local machine:

```sh
git clone https://github.com/yourusername/azure-template-elevated-access-removal.git
cd azure-template-elevated-access-removal
```

### Step 2: Customize Parameters

Edit the `parameters.json` file to customize the deployment parameters. Replace the `null` values with appropriate values for your environment:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "connections_azureautomation_name": {
      "value": "your-azureautomation-connection-name"
    },
    "connections_azuremonitorlogs_name": {
      "value": "your-azuremonitorlogs-connection-name"
    },
    "workflows_LA_ElevatedAccess_we_name": {
      "value": "your-logicapp-workflow-name"
    },
    "privateEndpoints_pep_aa_ElevatedAccess_we_name": {
      "value": "your-private-endpoint-name"
    },
    "virtualNetworks_vnet_logic_automation_we_hub_name": {
      "value": "your-virtual-network-name"
    },
    "privateDnsZones_privatelink_azure_automation_net_name": {
      "value": "your-private-dns-zone-name"
    },
    "automationAccounts_logic_ElevatedAccess_JIT_we_name": {
      "value": "your-automation-account-name"
    }
  }
}
```

### Step 3: Deploy the Template

Use the Azure CLI or Azure PowerShell to deploy the template.

#### Using Azure CLI

```sh
az deployment group create --resource-group <your-resource-group> --template-file template.json --parameters parameters.json
```

#### Using Azure PowerShell

```powershell
New-AzResourceGroupDeployment -ResourceGroupName <your-resource-group> -TemplateFile template.json -TemplateParameterFile parameters.json
```

## Resources Deployed

The template deploys the following resources:

- **Azure Automation Account**: With a system-assigned managed identity.
- **Private DNS Zone**: For private link.
- **Virtual Network**: With a specified address space and subnet.
- **Connections**: For Azure Automation and Azure Monitor Logs.
- **Connection Types**: For the Azure Automation account.
- **Jobs**: For the Azure Automation account.
- **Modules**: For the Azure Automation account.
- **Runbooks**: For the Azure Automation account.
- **Runtime Environments**: For the Azure Automation account.
- **Private Endpoint**: For secure access.
- **Private DNS Zone Groups**: To associate the private endpoint with the private DNS zone.
- **Logic App Workflow**: To automate the removal of elevated access.

## Logic App Workflow

The Logic App workflow is designed to:

1. Query Azure Monitor Logs for elevated access events.
2. Initialize a variable to store the output.
3. Iterate over the results and create a job in the Azure Automation account to remove elevated access.
4. Check the job output and update the variable.
5. Terminate the workflow if no successful removal is detected, or compose the variable for further use.

## License

This project is licensed under the MIT License. See the LICENSE file for details.

## Author

Sebastian Flæng Markdanner // chanceofsecurity.com

## Contact

For any questions or issues, please contact me on https://www.chanceofsecurity.com/contact or on any of my socials.
