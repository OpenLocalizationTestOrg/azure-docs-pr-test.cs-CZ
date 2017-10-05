---
title: "Nasazení prostředků Azure do několika skupin prostředků | Microsoft Docs"
description: "Ukazuje, jak mít více než jedné skupiny prostředků Azure během nasazení."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: tomfitz
ms.openlocfilehash: d8b041213b269775175a810e585103d3c538557f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-azure-resources-to-more-than-one-resource-group"></a><span data-ttu-id="e8def-103">Nasadit Azure prostředky do více než jedné skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="e8def-103">Deploy Azure resources to more than one resource group</span></span>

<span data-ttu-id="e8def-104">Zpravidla nasazujete, všechny prostředky ve vaší šablony jedna skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="e8def-104">Typically, you deploy all the resources in your template to a single resource group.</span></span> <span data-ttu-id="e8def-105">Existují však scénáře, ve které chcete nasadit sadu prostředků společně, ale je umístit do jiné skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="e8def-105">However, there are scenarios where you want to deploy a set of resources together but place them in different resource groups.</span></span> <span data-ttu-id="e8def-106">Můžete například nasazení zálohování virtuálního počítače pro Azure Site Recovery na skupinu samostatné prostředků a umístění.</span><span class="sxs-lookup"><span data-stu-id="e8def-106">For example, you may want to deploy the backup virtual machine for Azure Site Recovery to a separate resource group and location.</span></span> <span data-ttu-id="e8def-107">Resource Manager umožňuje použití vnořených šablon pro různé skupiny prostředků než použití v šabloně nadřazené skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="e8def-107">Resource Manager enables you to use nested templates to target different resource groups than the resource group used for the parent template.</span></span>

<span data-ttu-id="e8def-108">Skupina prostředků je kontejner životního cyklu pro aplikace a jeho kolekce prostředků.</span><span class="sxs-lookup"><span data-stu-id="e8def-108">The resource group is the lifecycle container for the application and its collection of resources.</span></span> <span data-ttu-id="e8def-109">Vytvořte skupinu prostředků mimo šablonu a zadejte skupinu prostředků pro během nasazení.</span><span class="sxs-lookup"><span data-stu-id="e8def-109">You create the resource group outside of the template, and specify the resource group to target during deployment.</span></span> <span data-ttu-id="e8def-110">Úvod do skupiny prostředků, najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e8def-110">For an introduction to resource groups, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

## <a name="example-template"></a><span data-ttu-id="e8def-111">Příklad šablony</span><span class="sxs-lookup"><span data-stu-id="e8def-111">Example template</span></span>

<span data-ttu-id="e8def-112">Pokud chcete zacílit na různé zdroje, musíte použít šablonu vnořené nebo propojené během nasazení.</span><span class="sxs-lookup"><span data-stu-id="e8def-112">To target a different resource, you must use a nested or linked template during deployment.</span></span> <span data-ttu-id="e8def-113">`Microsoft.Resources/deployments` Nabízí typ prostředku `resourceGroup` parametr, který umožňuje určit jinou skupinu prostředků pro vnořené nasazení.</span><span class="sxs-lookup"><span data-stu-id="e8def-113">The `Microsoft.Resources/deployments` resource type provides a `resourceGroup` parameter that enables you to specify a different resource group for the nested deployment.</span></span> <span data-ttu-id="e8def-114">Všechny skupiny prostředků musí existovat před spuštěním nasazení.</span><span class="sxs-lookup"><span data-stu-id="e8def-114">All the resource groups must exist before running the deployment.</span></span> <span data-ttu-id="e8def-115">Následující příklad nasadí dva účty úložiště – jeden ve skupině prostředků, které se zadávají během nasazení a po jednom v skupinu prostředků s názvem `crossResourceGroupDeployment`:</span><span class="sxs-lookup"><span data-stu-id="e8def-115">The following example deploys two storage accounts - one in the resource group specified during deployment, and one in a resource group named `crossResourceGroupDeployment`:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "StorageAccountName1": {
            "type": "string"
        },
        "StorageAccountName2": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "crossResourceGroupDeployment",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "name": "[parameters('StorageAccountName2')]",
                            "apiVersion": "2015-06-15",
                            "location": "West US",
                            "properties": {
                                "accountType": "Standard_LRS"
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('StorageAccountName1')]",
            "apiVersion": "2015-06-15",
            "location": "West US",
            "properties": {
                "accountType": "Standard_LRS"
            }
        }
    ]
}
```

<span data-ttu-id="e8def-116">Pokud nastavíte `resourceGroup` název skupiny prostředků, který ještě neexistuje, nasazení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="e8def-116">If you set `resourceGroup` to the name of a resource group that does not exist, the deployment fails.</span></span> <span data-ttu-id="e8def-117">Pokud nezadáte hodnotu `resourceGroup`, správce prostředků používá nadřazené skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="e8def-117">If you do not provide a value for `resourceGroup`, Resource Manager uses the parent resource group.</span></span>  

## <a name="deploy-the-template"></a><span data-ttu-id="e8def-118">Nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="e8def-118">Deploy the template</span></span>

<span data-ttu-id="e8def-119">Pokud chcete nasadit šablonu příklad, můžete portálu, prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e8def-119">To deploy the example template, you can use the portal, Azure PowerShell, or Azure CLI.</span></span> <span data-ttu-id="e8def-120">Pro prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure musíte použít verzi z může 2017 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e8def-120">For Azure PowerShell or Azure CLI, you must use a release from May 2017 or later.</span></span> <span data-ttu-id="e8def-121">Příklady předpokládají, že šablona byla uložena místně jako soubor s názvem **crossrgdeployment.json**.</span><span class="sxs-lookup"><span data-stu-id="e8def-121">The examples assume you have saved the template locally as a file named **crossrgdeployment.json**.</span></span>

<span data-ttu-id="e8def-122">Pro prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="e8def-122">For PowerShell:</span></span>

```powershell
Login-AzureRmAccount

New-AzureRmResourceGroup -Name mainResourceGroup -Location "South Central US"
New-AzureRmResourceGroup -Name crossResourceGroupDeployment -Location "Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName mainResourceGroup `
  -TemplateFile c:\MyTemplates\crossrgdeployment.json
```

<span data-ttu-id="e8def-123">Pokud používáte Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="e8def-123">For Azure CLI:</span></span>

```azurecli
az login

az group create --name mainResourceGroup --location "South Central US"
az group create --name crossResourceGroupDeployment --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group mainResourceGroup \
    --template-file crossrgdeployment.json
```

<span data-ttu-id="e8def-124">Po dokončení nasazení uvidíte dvě skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="e8def-124">After deployment completes, you see two resource groups.</span></span> <span data-ttu-id="e8def-125">Každé skupině prostředků obsahuje účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e8def-125">Each resource group contains a storage account.</span></span>

## <a name="use-resourcegroup-function"></a><span data-ttu-id="e8def-126">Pomocí funkce resourceGroup()</span><span class="sxs-lookup"><span data-stu-id="e8def-126">Use resourceGroup() function</span></span>

<span data-ttu-id="e8def-127">Pro různé nasazení skupiny prostředků, [resouceGroup() funkce](resource-group-template-functions-resource.md#resourcegroup) vyřeší různě v závislosti na tom, jak je zadat vnořené šablony.</span><span class="sxs-lookup"><span data-stu-id="e8def-127">For cross resource group deployments, the [resouceGroup() function](resource-group-template-functions-resource.md#resourcegroup) resolves differently based on how you specify the nested template.</span></span> 

<span data-ttu-id="e8def-128">Pokud vložit jednu šablonu v rámci jiné šablony resouceGroup() v šabloně vnořené přeloží do nadřazené skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="e8def-128">If you embed one template within another template, resouceGroup() in the nested template resolves to the parent resource group.</span></span> <span data-ttu-id="e8def-129">Šablonu embedded používá následující formát:</span><span class="sxs-lookup"><span data-stu-id="e8def-129">An embedded template uses the following format:</span></span>

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() refers to parent resource group
    }
}
```

<span data-ttu-id="e8def-130">Pokud jste odkaz na šablonu samostatné, resouceGroup() v šabloně propojené přeloží na skupině vnořeného prostředku.</span><span class="sxs-lookup"><span data-stu-id="e8def-130">If you link to a separate template, resouceGroup() in the linked template resolves to the nested resource group.</span></span> <span data-ttu-id="e8def-131">Propojené šablona používá následující formát:</span><span class="sxs-lookup"><span data-stu-id="e8def-131">A linked template uses the following format:</span></span>

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() in linked template refers to linked resource group
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="e8def-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e8def-132">Next steps</span></span>

* <span data-ttu-id="e8def-133">Chcete-li pochopit, jak definovat parametry v šabloně, přečtěte si téma [pochopit strukturu a syntaxe šablon Azure Resource Manager](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="e8def-133">To understand how to define parameters in your template, see [Understand the structure and syntax of Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="e8def-134">Tipy k řešení běžných chyb při nasazení, naleznete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="e8def-134">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="e8def-135">Informace o nasazení šablony, která vyžaduje tokenu SAS naleznete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="e8def-135">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>
