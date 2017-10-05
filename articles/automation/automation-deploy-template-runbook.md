---
title: "Nasadit šablonu Azure Resource Manager v runbooku automatizace Azure | Microsoft Docs"
description: "Jak nasadit šablonu Azure Resource Manageru uložená v úložišti Azure ze sady runbook"
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
keywords: "prostředí PowerShell, sady runbook, json, služby azure automation"
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 07/09/2017
ms.author: eslesar
ms.openlocfilehash: e511eee2f9eac3969b15ad3d45558dc7034f330a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-an-azure-resource-manager-template-in-an-azure-automation-powershell-runbook"></a><span data-ttu-id="392c9-104">Nasadit šablonu Azure Resource Manager v runbook služby Azure Automation PowerShell</span><span class="sxs-lookup"><span data-stu-id="392c9-104">Deploy an Azure Resource Manager template in an Azure Automation PowerShell runbook</span></span>

<span data-ttu-id="392c9-105">Můžete napsat [prostředí PowerShell Azure Automation runbook](automation-first-runbook-textual-powershell.md) které nasazuje prostředek služby Azure pomocí [šablony Azure Resource Management](../azure-resource-manager/resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="392c9-105">You can write an [Azure Automation PowerShell runbook](automation-first-runbook-textual-powershell.md) that deploys an Azure resource by using an [Azure Resource Management template](../azure-resource-manager/resource-manager-create-first-template.md).</span></span>

<span data-ttu-id="392c9-106">Díky tomu je možné automatizovat nasazení prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="392c9-106">By doing this, you can automate deployment of Azure resources.</span></span> <span data-ttu-id="392c9-107">Je možné uchovávat vaše šablony správce prostředků v centrální a zabezpečené umístění, třeba úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="392c9-107">You can maintain your Resource Manager templates in a central,secure location such as Azure Storage.</span></span>

<span data-ttu-id="392c9-108">V tomto tématu, vytvoříme runbook prostředí PowerShell, která používá šablonu Resource Manageru uložená v [Azure Storage](../storage/common/storage-introduction.md) nasadit nový účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="392c9-108">In this topic, we create a PowerShell runbook that uses an Resource Manager template stored in [Azure Storage](../storage/common/storage-introduction.md) to deploy a new Azure Storage account.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="392c9-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="392c9-109">Prerequisites</span></span>

<span data-ttu-id="392c9-110">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="392c9-110">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="392c9-111">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="392c9-111">Azure subscription.</span></span> <span data-ttu-id="392c9-112">Pokud nemáte účet, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo <a href="/pricing/free-account/" target="_blank">[si zaregistrovat bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="392c9-112">If you don't have one yet, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or <a href="/pricing/free-account/" target="_blank">[sign up for a free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="392c9-113">[Účet Automation](automation-sec-configure-azure-runas-account.md), abyste si mohli runbook podržet a mohli ověřovat prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="392c9-113">[Automation account](automation-sec-configure-azure-runas-account.md) to hold the runbook and authenticate to Azure resources.</span></span>  <span data-ttu-id="392c9-114">Tento účet musí mít oprávnění ke spuštění a zastavení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="392c9-114">This account must have permission to start and stop the virtual machine.</span></span>
* <span data-ttu-id="392c9-115">[Účet služby Azure Storage](../storage/common/storage-create-storage-account.md) pro uložení šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="392c9-115">[Azure Storage account](../storage/common/storage-create-storage-account.md) in which to store the Resource Manager template</span></span>
* <span data-ttu-id="392c9-116">Azure Powershell nainstalovaný v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="392c9-116">Azure Powershell installed on a local machine.</span></span> <span data-ttu-id="392c9-117">V tématu [nainstalovat a nakonfigurovat Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) informace o tom, jak získat prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="392c9-117">See [Install and configure Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) for information about how to get Azure PowerShell.</span></span>

## <a name="create-the-resource-manager-template"></a><span data-ttu-id="392c9-118">Vytvoření šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="392c9-118">Create the Resource Manager template</span></span>

<span data-ttu-id="392c9-119">V tomto příkladu používáme šablonu Resource Manager, která nasadí nový účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="392c9-119">For this example, we use an Resource Manager template that deploys a new Azure Storage account.</span></span>

<span data-ttu-id="392c9-120">V textovém editoru zkopírujte následující text:</span><span class="sxs-lookup"><span data-stu-id="392c9-120">In a text editor, copy the following text:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
    }
  ],
  "outputs": {
      "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
      }
  }
}
```

<span data-ttu-id="392c9-121">Uložte soubor místně jako `TemplateTest.json`.</span><span class="sxs-lookup"><span data-stu-id="392c9-121">Save the file locally as `TemplateTest.json`.</span></span>

## <a name="save-the-resource-manager-template-in-azure-storage"></a><span data-ttu-id="392c9-122">Uložení šablony Resource Manageru v Azure Storage</span><span class="sxs-lookup"><span data-stu-id="392c9-122">Save the Resource Manager template in Azure Storage</span></span>

<span data-ttu-id="392c9-123">Teď nemůžeme použít PowerShell k vytvoření sdílené složky Azure Storage a odeslat `TemplateTest.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="392c9-123">Now we use PowerShell to create an Azure Storage file share and upload the `TemplateTest.json` file.</span></span>
<span data-ttu-id="392c9-124">Pokyny, jak vytvořit soubor sdílet a nahrát soubor na portálu Azure, najdete v části [Začínáme s Azure File storage ve Windows](../storage/files/storage-dotnet-how-to-use-files.md).</span><span class="sxs-lookup"><span data-stu-id="392c9-124">For instructions on how to create a file share and upload a file in the Azure portal, see [Get started with Azure File storage on Windows](../storage/files/storage-dotnet-how-to-use-files.md).</span></span>

<span data-ttu-id="392c9-125">Spusťte PowerShell na místním počítači a spusťte následující příkaz pro vytvoření sdílené složky a odeslat šablony Resource Manageru do této sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="392c9-125">Launch PowerShell on your local machine, and run the following commands to create a file share and upload the Resource Manager template to that file share.</span></span>

```powershell
# Login to Azure
Login-AzureRmAccount

# Get the access key for your storage account
$key = Get-AzureRmStorageAccountKey -ResourceGroupName 'MyAzureAccount' -Name 'MyStorageAccount'

# Create an Azure Storage context using the first access key
$context = New-AzureStorageContext -StorageAccountName 'MyStorageAccount' -StorageAccountKey $key[0].value

# Create a file share named 'resource-templates' in your Azure Storage account
$fileShare = New-AzureStorageShare -Name 'resource-templates' -Context $context

# Add the TemplateTest.json file to the new file share
# "TemplatePath" is the path where you saved the TemplateTest.json file
$templateFile = 'C:\TemplatePath'
Set-AzureStorageFileContent -ShareName $fileShare.Name -Context $context -Source $templateFile
```

## <a name="create-the-powershell-runbook-script"></a><span data-ttu-id="392c9-126">Vytvoření sady runbook skript prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="392c9-126">Create the PowerShell runbook script</span></span>

<span data-ttu-id="392c9-127">Teď vytvoříme skript prostředí PowerShell, který získá `TemplateTest.json` souborů ze služby Azure Storage a nasadí šablonu, kterou chcete vytvořit nový účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="392c9-127">Now we create a PowerShell script that gets the `TemplateTest.json` file from Azure Storage and deploys the template to create a new Azure Storage account.</span></span>

<span data-ttu-id="392c9-128">V textovém editoru vložte následující text:</span><span class="sxs-lookup"><span data-stu-id="392c9-128">In a text editor, paste the following text:</span></span>

```powershell
param (
    [Parameter(Mandatory=$true)]
    [string]
    $ResourceGroupName,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageAccountName,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageAccountKey,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageFileName
)



# Authenticate to Azure if running from Azure Automation
$ServicePrincipalConnection = Get-AutomationConnection -Name "AzureRunAsConnection"
Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint | Write-Verbose

#Set the parameter values for the Resource Manager template
$Parameters = @{
    "storageAccountType"="Standard_LRS"
    }

# Create a new context
$Context = New-AzureStorageContext -StorageAccountKey $StorageAccountKey

Get-AzureStorageFileContent -ShareName 'resource-templates' -Context $Context -path 'TemplateTest.json' -Destination 'C:\Temp'

$TemplateFile = Join-Path -Path 'C:\Temp' -ChildPath $StorageFileName

# Deploy the storage account
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterObject $Parameters 
``` 

<span data-ttu-id="392c9-129">Uložte soubor místně jako `DeployTemplate.ps1`.</span><span class="sxs-lookup"><span data-stu-id="392c9-129">Save the file locally as `DeployTemplate.ps1`.</span></span>

## <a name="import-and-publish-the-runbook-into-your-azure-automation-account"></a><span data-ttu-id="392c9-130">Import a publikovat sadu runbook k účtu Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="392c9-130">Import and publish the runbook into your Azure Automation account</span></span>

<span data-ttu-id="392c9-131">Nyní jsme naimportovat sady runbook do účtu Azure Automation pomocí prostředí PowerShell a pak publikovat sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="392c9-131">Now we use PowerShell to import the runbook into your Azure Automation account, and then publish the runbook.</span></span>
<span data-ttu-id="392c9-132">Informace o tom, jak importovat a publikování sady runbook na portálu Azure najdete v tématu [vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="392c9-132">For information about how to import and publish a runbook in the Azure portal, see [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md).</span></span>

<span data-ttu-id="392c9-133">Chcete-li importovat `DeployTemplate.ps1` do účtu Automation jako runbook prostředí PowerShell, spusťte následující příkazy prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="392c9-133">To import `DeployTemplate.ps1` into your Automation account as a PowerShell runbook, run the following PowerShell commands:</span></span>

```powershell
# MyPath is the path where you saved DeployTemplate.ps1
# MyResourceGroup is the name of the Azure ResourceGroup that contains your Azure Automation account
# MyAutomationAccount is the name of your Automation account
$importParams = @{
    Path = 'C:\MyPath\DeployTemplate.ps1'
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Type = 'PowerShell'
}
Import-AzureRmAutomationRunbook @

# Publish the runbook
$publishParams = @{
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Name = 'DeployTemplate'
}
Publish-AzureRmAutomationRunbook @publishParams
```

## <a name="start-the-runbook"></a><span data-ttu-id="392c9-134">Spuštění runbooku</span><span class="sxs-lookup"><span data-stu-id="392c9-134">Start the runbook</span></span>

<span data-ttu-id="392c9-135">Nyní jsme spuštění runbooku voláním [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook?view=azurermps-4.1.0) rutiny.</span><span class="sxs-lookup"><span data-stu-id="392c9-135">Now we start the runbook by calling the [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook?view=azurermps-4.1.0) cmdlet.</span></span>

<span data-ttu-id="392c9-136">Informace o tom, jak spuštění sady runbook na portálu Azure najdete v tématu [spuštění sady runbook ve službě Azure Automation](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="392c9-136">For information about how to start a runbook in the Azure portal, see [Starting a runbook in Azure Automation](automation-starting-a-runbook.md).</span></span>

<span data-ttu-id="392c9-137">V konzole PowerShell, spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="392c9-137">Run the following commands in the PowerShell console:</span></span>

```powershell
# Set up the parameters for the runbook
$runbookParams = @{
    ResourceGroupName = 'MyResourceGroup'
    StorageAccountName = 'MyStorageAccount'
    StorageAccountKey = $key[0].Value # We got this key earlier
    StorageFileName = 'TemplateTest.json' 
}

# Set up parameters for the Start-AzureRmAutomationRunbook cmdlet
$startParams = @{
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Name = 'DeployTemplate'
    Parameters = $runbookParams
}

# Start the runbook
$job = Start-AzureRmAutomationRunbook @startParams
```

<span data-ttu-id="392c9-138">Sada runbook běží, a její stav můžete zkontrolovat spuštěním `$job.Status`.</span><span class="sxs-lookup"><span data-stu-id="392c9-138">The runbook runs, and you can check its status by running `$job.Status`.</span></span>

<span data-ttu-id="392c9-139">Sada runbook získá šablony Resource Manageru a použije ho k nasazení nového účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="392c9-139">The runbook gets the Resource Manager template and uses it to deploy a new Azure Storage account.</span></span>
<span data-ttu-id="392c9-140">Můžete zjistit, zda byl vytvořen nový účet úložiště tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="392c9-140">You can see that the new storage account was created by running the following command:</span></span>
```powershell
Get-AzureRmStorageAccount
```

## <a name="summary"></a><span data-ttu-id="392c9-141">Souhrn</span><span class="sxs-lookup"><span data-stu-id="392c9-141">Summary</span></span>

<span data-ttu-id="392c9-142">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="392c9-142">That's it!</span></span> <span data-ttu-id="392c9-143">Teď můžete použít šablony Azure Automation a úložiště Azure a Resource Manager k nasazení všech vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="392c9-143">Now you can use Azure Automation and Azure Storage, and Resource Manager templates to deploy all your Azure resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="392c9-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="392c9-144">Next steps</span></span>

* <span data-ttu-id="392c9-145">Další informace o šablonách Resource Manager najdete v tématu [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="392c9-145">To learn more about Resource Manager templates, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md)</span></span>
* <span data-ttu-id="392c9-146">Začínáme s Azure Storage, najdete v tématu [Úvod do Azure Storage](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="392c9-146">To get started with Azure Storage, see [Introduction to Azure Storage](../storage/common/storage-introduction.md).</span></span>
* <span data-ttu-id="392c9-147">Další užitečné runbooků služeb automatizace Azure, najdete v tématu [Galerie Runbooků a modulů pro Azure Automation](automation-runbook-gallery.md).</span><span class="sxs-lookup"><span data-stu-id="392c9-147">To find other useful Azure Automation runbooks, see [Runbook and module galleries for Azure Automation](automation-runbook-gallery.md).</span></span>
* <span data-ttu-id="392c9-148">Další užitečné šablony Resource Manageru, najdete v tématu [šablon Azure rychlý start](https://azure.microsoft.com/resources/templates/)</span><span class="sxs-lookup"><span data-stu-id="392c9-148">To find other useful Resource Manager templates, see [Azure Quickstart Templates](https://azure.microsoft.com/resources/templates/)</span></span>
