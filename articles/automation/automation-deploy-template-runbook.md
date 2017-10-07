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
# <a name="deploy-an-azure-resource-manager-template-in-an-azure-automation-powershell-runbook"></a>Nasadit šablonu Azure Resource Manager v runbook služby Azure Automation PowerShell

Můžete napsat [prostředí PowerShell Azure Automation runbook](automation-first-runbook-textual-powershell.md) které nasazuje prostředek služby Azure pomocí [šablony Azure Resource Management](../azure-resource-manager/resource-manager-create-first-template.md).

Díky tomu je možné automatizovat nasazení prostředků Azure. Je možné uchovávat vaše šablony správce prostředků v centrální a zabezpečené umístění, třeba úložiště Azure.

V tomto tématu, vytvoříme runbook prostředí PowerShell, která používá šablonu Resource Manageru uložená v [Azure Storage](../storage/common/storage-introduction.md) toodeploy nový účet úložiště Azure.

## <a name="prerequisites"></a>Požadavky

toocomplete tohoto kurzu budete potřebovat hello následující:

* Předplatné Azure. Pokud nemáte účet, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo <a href="/pricing/free-account/" target="_blank">[si zaregistrovat bezplatný účet](https://azure.microsoft.com/free/).
* [Účet Automation](automation-sec-configure-azure-runas-account.md) toohold hello sady runbook a ověření tooAzure prostředky.  Tento účet musí mít oprávnění toostart a zastavit hello virtuální počítač.
* [Účet služby Azure Storage](../storage/common/storage-create-storage-account.md) v šablonu toostore hello Resource Manager
* Azure Powershell nainstalovaný v místním počítači. V tématu [nainstalovat a nakonfigurovat Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) informace o tooget prostředí Azure PowerShell.

## <a name="create-hello-resource-manager-template"></a>Vytvoření šablony Resource Manageru hello

V tomto příkladu používáme šablonu Resource Manager, která nasadí nový účet úložiště Azure.

V textovém editoru zkopírujte následující text hello:

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

Uložte soubor hello místně jako `TemplateTest.json`.

## <a name="save-hello-resource-manager-template-in-azure-storage"></a>Uložení šablony Resource Manageru hello ve službě Azure Storage

Pomocí prostředí PowerShell toocreate sdílené složky úložiště Azure a nahrát hello teď `TemplateTest.json` souboru.
Pokyny, jak toocreate soubor sdílet a odeslání souboru v hello portálu Azure, najdete v části [Začínáme s Azure File storage ve Windows](../storage/files/storage-dotnet-how-to-use-files.md).

Na místním počítači, spusťte PowerShell a spusťte následující příkazy toocreate sdílenou hello a nahrání hello Resource Manager šablony toothat sdílené složky.

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

## <a name="create-hello-powershell-runbook-script"></a>Vytvořte skript, runbook PowerShell hello

Teď vytvoříme skript prostředí PowerShell, který získá hello `TemplateTest.json` souborů ze služby Azure Storage a nasadí hello šablony toocreate nový účet úložiště Azure.

V textovém editoru vložte následující text hello:

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

Uložte soubor hello místně jako `DeployTemplate.ps1`.

## <a name="import-and-publish-hello-runbook-into-your-azure-automation-account"></a>Import a publikovat hello runbook k účtu Azure Automation.

Nyní jsme pomocí prostředí PowerShell tooimport hello runbook do účtu Azure Automation a pak publikovat hello runbook.
Informace o tooimport a publikování runbooku ve hello portálu Azure, najdete v části [vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md).

tooimport `DeployTemplate.ps1` do účtu Automation jako runbook Powershellu spusťte následující příkazy prostředí PowerShell hello:

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

## <a name="start-hello-runbook"></a>Spuštění sady runbook hello

Nyní jsme spustit hello runbook pomocí volání hello [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook?view=azurermps-4.1.0) rutiny.

Informace o tom, jak toostart sady runbook v hello portál Azure, najdete v části [spuštění sady runbook ve službě Azure Automation](automation-starting-a-runbook.md).

Spusťte následující příkazy v konzole PowerShell hello hello:

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

Hello spuštěna sada runbook a její stav můžete zkontrolovat spuštěním `$job.Status`.

Hello runbook získá hello šablony Resource Manageru a používá je toodeploy nový účet úložiště Azure.
Můžete zjistit, že byl vytvořen nový účet úložiště hello tak, že spustíte následující příkaz hello:
```powershell
Get-AzureRmStorageAccount
```

## <a name="summary"></a>Souhrn

A to je vše! Teď můžete použít Azure Automation a Azure Storage a toodeploy šablony Resource Manageru všech vašich prostředků Azure.

## <a name="next-steps"></a>Další kroky

* toolearn Další informace o šablonách Resource Manageru, najdete v části [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md)
* tooget začít s Azure Storage, najdete v části [Úvod tooAzure úložiště](../storage/common/storage-introduction.md).
* toofind najdete v části Další užitečné Azure Automation runbook [Galerie Runbooků a modulů pro Azure Automation](automation-runbook-gallery.md).
* toofind další užitečné šablony Resource Manageru, najdete v části [šablon Azure rychlý start](https://azure.microsoft.com/resources/templates/)

