---
title: "aaaDeploy šablonu Azure Resource Manager v runbooku automatizace Azure | Microsoft Docs"
description: "Jak toodeploy šablonu Azure Resource Manageru uložená v úložišti Azure ze sady runbook"
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
ms.openlocfilehash: f489a8e8635a48f5a6a2f1a88e1c803f56f01832
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-azure-resource-manager-template-in-an-azure-automation-powershell-runbook"></a><span data-ttu-id="b8573-104">Nasadit šablonu Azure Resource Manager v runbook služby Azure Automation PowerShell</span><span class="sxs-lookup"><span data-stu-id="b8573-104">Deploy an Azure Resource Manager template in an Azure Automation PowerShell runbook</span></span>

<span data-ttu-id="b8573-105">Můžete napsat [prostředí PowerShell Azure Automation runbook](automation-first-runbook-textual-powershell.md) které nasazuje prostředek služby Azure pomocí [šablony Azure Resource Management](../azure-resource-manager/resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="b8573-105">You can write an [Azure Automation PowerShell runbook](automation-first-runbook-textual-powershell.md) that deploys an Azure resource by using an [Azure Resource Management template](../azure-resource-manager/resource-manager-create-first-template.md).</span></span>

<span data-ttu-id="b8573-106">Díky tomu je možné automatizovat nasazení prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="b8573-106">By doing this, you can automate deployment of Azure resources.</span></span> <span data-ttu-id="b8573-107">Je možné uchovávat vaše šablony správce prostředků v centrální a zabezpečené umístění, třeba úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b8573-107">You can maintain your Resource Manager templates in a central,secure location such as Azure Storage.</span></span>

<span data-ttu-id="b8573-108">V tomto tématu, vytvoříme runbook prostředí PowerShell, která používá šablonu Resource Manageru uložená v [Azure Storage](../storage/common/storage-introduction.md) toodeploy nový účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b8573-108">In this topic, we create a PowerShell runbook that uses an Resource Manager template stored in [Azure Storage](../storage/common/storage-introduction.md) toodeploy a new Azure Storage account.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b8573-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b8573-109">Prerequisites</span></span>

<span data-ttu-id="b8573-110">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="b8573-110">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="b8573-111">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="b8573-111">Azure subscription.</span></span> <span data-ttu-id="b8573-112">Pokud nemáte účet, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo <a href="/pricing/free-account/" target="_blank">[si zaregistrovat bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="b8573-112">If you don't have one yet, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or <a href="/pricing/free-account/" target="_blank">[sign up for a free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="b8573-113">[Účet Automation](automation-sec-configure-azure-runas-account.md) toohold hello sady runbook a ověření tooAzure prostředky.</span><span class="sxs-lookup"><span data-stu-id="b8573-113">[Automation account](automation-sec-configure-azure-runas-account.md) toohold hello runbook and authenticate tooAzure resources.</span></span>  <span data-ttu-id="b8573-114">Tento účet musí mít oprávnění toostart a zastavit hello virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="b8573-114">This account must have permission toostart and stop hello virtual machine.</span></span>
* <span data-ttu-id="b8573-115">[Účet služby Azure Storage](../storage/common/storage-create-storage-account.md) v šablonu toostore hello Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b8573-115">[Azure Storage account](../storage/common/storage-create-storage-account.md) in which toostore hello Resource Manager template</span></span>
* <span data-ttu-id="b8573-116">Azure Powershell nainstalovaný v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="b8573-116">Azure Powershell installed on a local machine.</span></span> <span data-ttu-id="b8573-117">V tématu [nainstalovat a nakonfigurovat Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) informace o tooget prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b8573-117">See [Install and configure Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) for information about how tooget Azure PowerShell.</span></span>

## <a name="create-hello-resource-manager-template"></a><span data-ttu-id="b8573-118">Vytvoření šablony Resource Manageru hello</span><span class="sxs-lookup"><span data-stu-id="b8573-118">Create hello Resource Manager template</span></span>

<span data-ttu-id="b8573-119">V tomto příkladu používáme šablonu Resource Manager, která nasadí nový účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b8573-119">For this example, we use an Resource Manager template that deploys a new Azure Storage account.</span></span>

<span data-ttu-id="b8573-120">V textovém editoru zkopírujte následující text hello:</span><span class="sxs-lookup"><span data-stu-id="b8573-120">In a text editor, copy hello following text:</span></span>

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

<span data-ttu-id="b8573-121">Uložte soubor hello místně jako `TemplateTest.json`.</span><span class="sxs-lookup"><span data-stu-id="b8573-121">Save hello file locally as `TemplateTest.json`.</span></span>

## <a name="save-hello-resource-manager-template-in-azure-storage"></a><span data-ttu-id="b8573-122">Uložení šablony Resource Manageru hello ve službě Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b8573-122">Save hello Resource Manager template in Azure Storage</span></span>

<span data-ttu-id="b8573-123">Pomocí prostředí PowerShell toocreate sdílené složky úložiště Azure a nahrát hello teď `TemplateTest.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="b8573-123">Now we use PowerShell toocreate an Azure Storage file share and upload hello `TemplateTest.json` file.</span></span>
<span data-ttu-id="b8573-124">Pokyny, jak toocreate soubor sdílet a odeslání souboru v hello portálu Azure, najdete v části [Začínáme s Azure File storage ve Windows](../storage/files/storage-dotnet-how-to-use-files.md).</span><span class="sxs-lookup"><span data-stu-id="b8573-124">For instructions on how toocreate a file share and upload a file in hello Azure portal, see [Get started with Azure File storage on Windows](../storage/files/storage-dotnet-how-to-use-files.md).</span></span>

<span data-ttu-id="b8573-125">Na místním počítači, spusťte PowerShell a spusťte následující příkazy toocreate sdílenou hello a nahrání hello Resource Manager šablony toothat sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="b8573-125">Launch PowerShell on your local machine, and run hello following commands toocreate a file share and upload hello Resource Manager template toothat file share.</span></span>

```powershell
# Login tooAzure
Login-AzureRmAccount

# Get hello access key for your storage account
$key = Get-AzureRmStorageAccountKey -ResourceGroupName 'MyAzureAccount' -Name 'MyStorageAccount'

# Create an Azure Storage context using hello first access key
$context = New-AzureStorageContext -StorageAccountName 'MyStorageAccount' -StorageAccountKey $key[0].value

# Create a file share named 'resource-templates' in your Azure Storage account
$fileShare = New-AzureStorageShare -Name 'resource-templates' -Context $context

# Add hello TemplateTest.json file toohello new file share
# "TemplatePath" is hello path where you saved hello TemplateTest.json file
$templateFile = 'C:\TemplatePath'
Set-AzureStorageFileContent -ShareName $fileShare.Name -Context $context -Source $templateFile
```

## <a name="create-hello-powershell-runbook-script"></a><span data-ttu-id="b8573-126">Vytvořte skript, runbook PowerShell hello</span><span class="sxs-lookup"><span data-stu-id="b8573-126">Create hello PowerShell runbook script</span></span>

<span data-ttu-id="b8573-127">Teď vytvoříme skript prostředí PowerShell, který získá hello `TemplateTest.json` souborů ze služby Azure Storage a nasadí hello šablony toocreate nový účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b8573-127">Now we create a PowerShell script that gets hello `TemplateTest.json` file from Azure Storage and deploys hello template toocreate a new Azure Storage account.</span></span>

<span data-ttu-id="b8573-128">V textovém editoru vložte následující text hello:</span><span class="sxs-lookup"><span data-stu-id="b8573-128">In a text editor, paste hello following text:</span></span>

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



# Authenticate tooAzure if running from Azure Automation
$ServicePrincipalConnection = Get-AutomationConnection -Name "AzureRunAsConnection"
Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint | Write-Verbose

#Set hello parameter values for hello Resource Manager template
$Parameters = @{
    "storageAccountType"="Standard_LRS"
    }

# Create a new context
$Context = New-AzureStorageContext -StorageAccountKey $StorageAccountKey

Get-AzureStorageFileContent -ShareName 'resource-templates' -Context $Context -path 'TemplateTest.json' -Destination 'C:\Temp'

$TemplateFile = Join-Path -Path 'C:\Temp' -ChildPath $StorageFileName

# Deploy hello storage account
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterObject $Parameters 
``` 

<span data-ttu-id="b8573-129">Uložte soubor hello místně jako `DeployTemplate.ps1`.</span><span class="sxs-lookup"><span data-stu-id="b8573-129">Save hello file locally as `DeployTemplate.ps1`.</span></span>

## <a name="import-and-publish-hello-runbook-into-your-azure-automation-account"></a><span data-ttu-id="b8573-130">Import a publikovat hello runbook k účtu Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="b8573-130">Import and publish hello runbook into your Azure Automation account</span></span>

<span data-ttu-id="b8573-131">Nyní jsme pomocí prostředí PowerShell tooimport hello runbook do účtu Azure Automation a pak publikovat hello runbook.</span><span class="sxs-lookup"><span data-stu-id="b8573-131">Now we use PowerShell tooimport hello runbook into your Azure Automation account, and then publish hello runbook.</span></span>
<span data-ttu-id="b8573-132">Informace o tooimport a publikování runbooku ve hello portálu Azure, najdete v části [vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="b8573-132">For information about how tooimport and publish a runbook in hello Azure portal, see [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md).</span></span>

<span data-ttu-id="b8573-133">tooimport `DeployTemplate.ps1` do účtu Automation jako runbook Powershellu spusťte následující příkazy prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="b8573-133">tooimport `DeployTemplate.ps1` into your Automation account as a PowerShell runbook, run hello following PowerShell commands:</span></span>

```powershell
# MyPath is hello path where you saved DeployTemplate.ps1
# MyResourceGroup is hello name of hello Azure ResourceGroup that contains your Azure Automation account
# MyAutomationAccount is hello name of your Automation account
$importParams = @{
    Path = 'C:\MyPath\DeployTemplate.ps1'
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Type = 'PowerShell'
}
Import-AzureRmAutomationRunbook @

# Publish hello runbook
$publishParams = @{
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Name = 'DeployTemplate'
}
Publish-AzureRmAutomationRunbook @publishParams
```

## <a name="start-hello-runbook"></a><span data-ttu-id="b8573-134">Spuštění sady runbook hello</span><span class="sxs-lookup"><span data-stu-id="b8573-134">Start hello runbook</span></span>

<span data-ttu-id="b8573-135">Nyní jsme spustit hello runbook pomocí volání hello [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook?view=azurermps-4.1.0) rutiny.</span><span class="sxs-lookup"><span data-stu-id="b8573-135">Now we start hello runbook by calling hello [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook?view=azurermps-4.1.0) cmdlet.</span></span>

<span data-ttu-id="b8573-136">Informace o tom, jak toostart sady runbook v hello portál Azure, najdete v části [spuštění sady runbook ve službě Azure Automation](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="b8573-136">For information about how toostart a runbook in hello Azure portal, see [Starting a runbook in Azure Automation](automation-starting-a-runbook.md).</span></span>

<span data-ttu-id="b8573-137">Spusťte následující příkazy v konzole PowerShell hello hello:</span><span class="sxs-lookup"><span data-stu-id="b8573-137">Run hello following commands in hello PowerShell console:</span></span>

```powershell
# Set up hello parameters for hello runbook
$runbookParams = @{
    ResourceGroupName = 'MyResourceGroup'
    StorageAccountName = 'MyStorageAccount'
    StorageAccountKey = $key[0].Value # We got this key earlier
    StorageFileName = 'TemplateTest.json' 
}

# Set up parameters for hello Start-AzureRmAutomationRunbook cmdlet
$startParams = @{
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Name = 'DeployTemplate'
    Parameters = $runbookParams
}

# Start hello runbook
$job = Start-AzureRmAutomationRunbook @startParams
```

<span data-ttu-id="b8573-138">Hello spuštěna sada runbook a její stav můžete zkontrolovat spuštěním `$job.Status`.</span><span class="sxs-lookup"><span data-stu-id="b8573-138">hello runbook runs, and you can check its status by running `$job.Status`.</span></span>

<span data-ttu-id="b8573-139">Hello runbook získá hello šablony Resource Manageru a používá je toodeploy nový účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="b8573-139">hello runbook gets hello Resource Manager template and uses it toodeploy a new Azure Storage account.</span></span>
<span data-ttu-id="b8573-140">Můžete zjistit, že byl vytvořen nový účet úložiště hello tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="b8573-140">You can see that hello new storage account was created by running hello following command:</span></span>
```powershell
Get-AzureRmStorageAccount
```

## <a name="summary"></a><span data-ttu-id="b8573-141">Souhrn</span><span class="sxs-lookup"><span data-stu-id="b8573-141">Summary</span></span>

<span data-ttu-id="b8573-142">A to je vše!</span><span class="sxs-lookup"><span data-stu-id="b8573-142">That's it!</span></span> <span data-ttu-id="b8573-143">Teď můžete použít Azure Automation a Azure Storage a toodeploy šablony Resource Manageru všech vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="b8573-143">Now you can use Azure Automation and Azure Storage, and Resource Manager templates toodeploy all your Azure resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b8573-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b8573-144">Next steps</span></span>

* <span data-ttu-id="b8573-145">toolearn Další informace o šablonách Resource Manageru, najdete v části [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="b8573-145">toolearn more about Resource Manager templates, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md)</span></span>
* <span data-ttu-id="b8573-146">tooget začít s Azure Storage, najdete v části [Úvod tooAzure úložiště](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b8573-146">tooget started with Azure Storage, see [Introduction tooAzure Storage](../storage/common/storage-introduction.md).</span></span>
* <span data-ttu-id="b8573-147">toofind najdete v části Další užitečné Azure Automation runbook [Galerie Runbooků a modulů pro Azure Automation](automation-runbook-gallery.md).</span><span class="sxs-lookup"><span data-stu-id="b8573-147">toofind other useful Azure Automation runbooks, see [Runbook and module galleries for Azure Automation](automation-runbook-gallery.md).</span></span>
* <span data-ttu-id="b8573-148">toofind další užitečné šablony Resource Manageru, najdete v části [šablon Azure rychlý start](https://azure.microsoft.com/resources/templates/)</span><span class="sxs-lookup"><span data-stu-id="b8573-148">toofind other useful Resource Manager templates, see [Azure Quickstart Templates](https://azure.microsoft.com/resources/templates/)</span></span>

