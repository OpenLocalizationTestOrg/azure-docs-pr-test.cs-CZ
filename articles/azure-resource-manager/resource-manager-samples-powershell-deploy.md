---
title: "aaaAzure ukázkový skript prostředí PowerShell - nasazení šablony | Microsoft Docs"
description: "Ukázkový skript pro nasazení šablonu Azure Resource Manager."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: 536b8ccecad4ed8a4c4a4139c6bf4600e2eb9405
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-manager-template-deployment---powershell-script"></a><span data-ttu-id="eb0cc-103">Nasazení šablony Azure Resource Manager - skript prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="eb0cc-103">Azure Resource Manager template deployment - PowerShell script</span></span>

<span data-ttu-id="eb0cc-104">Tento skript nasadí skupiny prostředků tooa šablony správce prostředků ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-104">This script deploys a Resource Manager template tooa resource group in your subscription.</span></span>

[!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="eb0cc-105">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="eb0cc-105">Sample script</span></span>

```powershell
<#
 .SYNOPSIS
    Deploys a template tooAzure

 .DESCRIPTION
    Deploys an Azure Resource Manager template
#>

param (
    [Parameter(Mandatory)]
    #hello subscription id where hello template will be deployed.
    [string]$SubscriptionId,  

    [Parameter(Mandatory)]
    #hello resource group where hello template will be deployed. Can be hello name of an existing or a new resource group.
    [string]$ResourceGroupName, 

    #Optional, a resource group location. If specified, will try toocreate a new resource group in this location. If not specified, assumes resource group is existing.
    [string]$ResourceGroupLocation, 

    #hello deployment name.
    [Parameter(Mandatory)]
    [string]$DeploymentName,    

    #Path toohello template file. Defaults tootemplate.json.
    [string]$TemplateFilePath = "template.json",  

    #Path toohello parameters file. Defaults tooparameters.json. If file is not found, will prompt for parameter values based on template.
    [string]$ParametersFilePath = "parameters.json"
)

$ErrorActionPreference = "Stop"

# Login tooAzure and select subscription
Write-Output "Logging in"
Login-AzureRmAccount
Write-Output "Selecting subscription '$SubscriptionId'"
Select-AzureRmSubscription -SubscriptionID $SubscriptionId

# Create or check for existing resource group
$resourceGroup = Get-AzureRmResourceGroup -Name $ResourceGroupName -ErrorAction SilentlyContinue
if ( -not $ResourceGroup ) {
    Write-Output "Could not find resource group '$ResourceGroupName' - will create it"
    if ( -not $ResourceGroupLocation ) {
        $ResourceGroupLocation = Read-Host -Prompt 'Enter location for resource group'
    }
    Write-Output "Creating resource group '$ResourceGroupName' in location '$ResourceGroupLocation'"
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else {
    Write-Output "Using existing resource group '$ResourceGroupName'"
}

# Start hello deployment
Write-Output "Starting deployment"
if ( Test-Path $ParametersFilePath ) {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFilePath -TemplateParameterFile $ParametersFilePath
}
else {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFilePath
}
``` 

## <a name="clean-up-deployment"></a><span data-ttu-id="eb0cc-106">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="eb0cc-106">Clean up deployment</span></span> 

<span data-ttu-id="eb0cc-107">Hello spusťte následující příkaz, který skupinu prostředků hello tooremove a všechny její prostředky.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-107">Run hello following command tooremove hello resource group and all its resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="eb0cc-108">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="eb0cc-108">Script explanation</span></span>

<span data-ttu-id="eb0cc-109">Tento skript používá hello následující příkazy toocreate hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-109">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="eb0cc-110">Každou položku v tabulce hello propojí toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-110">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="eb0cc-111">Příkaz</span><span class="sxs-lookup"><span data-stu-id="eb0cc-111">Command</span></span> | <span data-ttu-id="eb0cc-112">Poznámky</span><span class="sxs-lookup"><span data-stu-id="eb0cc-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="eb0cc-113">Registrace AzureRmResourceProvider</span><span class="sxs-lookup"><span data-stu-id="eb0cc-113">Register-AzureRmResourceProvider</span></span>](/powershell/module/azurerm.resources/register-azurermresourceprovider) | <span data-ttu-id="eb0cc-114">Registruje zprostředkovatele prostředků, tak jeho typy prostředků může být nasazený tooyour předplatné.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-114">Registers a resource provider so its resource types can be deployed tooyour subscription.</span></span>  |
| [<span data-ttu-id="eb0cc-115">Get-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="eb0cc-115">Get-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/get-azurermresourcegroup) | <span data-ttu-id="eb0cc-116">Získá skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-116">Gets resource groups.</span></span>  |
| [<span data-ttu-id="eb0cc-117">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="eb0cc-117">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="eb0cc-118">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="eb0cc-119">Nové AzureRmResourceGroupDeployment</span><span class="sxs-lookup"><span data-stu-id="eb0cc-119">New-AzureRmResourceGroupDeployment</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) | <span data-ttu-id="eb0cc-120">Přidá skupinu prostředků tooa nasazení Azure.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-120">Adds an Azure deployment tooa resource group.</span></span>  |
| [<span data-ttu-id="eb0cc-121">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="eb0cc-121">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="eb0cc-122">Odebere skupinu prostředků a všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="eb0cc-122">Removes a resource group and all resources contained within.</span></span> |



## <a name="next-steps"></a><span data-ttu-id="eb0cc-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eb0cc-123">Next steps</span></span>
* <span data-ttu-id="eb0cc-124">Úvod toodeploying šablony, najdete v části [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="eb0cc-124">For an introduction toodeploying templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="eb0cc-125">Informace o nasazení šablony, která vyžaduje tokenu SAS naleznete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="eb0cc-125">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>
* <span data-ttu-id="eb0cc-126">toodefine parametry v šabloně, najdete v části [vytváření šablon](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="eb0cc-126">toodefine parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="eb0cc-127">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="eb0cc-127">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

