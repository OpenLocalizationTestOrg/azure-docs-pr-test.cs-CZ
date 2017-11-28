---
title: "skupiny prostředků toomultiple aaaDeploy prostředky Azure | Microsoft Docs"
description: "Ukazuje, jak tootarget více než jeden prostředek Azure skupiny během nasazení."
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
ms.openlocfilehash: 93a39a26e0ca18dfcb5c6e8de95c38a64186d6de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-azure-resources-toomore-than-one-resource-group"></a><span data-ttu-id="e978c-103">Nasazení prostředků Azure toomore než jedné skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="e978c-103">Deploy Azure resources toomore than one resource group</span></span>

<span data-ttu-id="e978c-104">Zpravidla nasazujete, všechny prostředky hello ve vaší skupině pro jediný zdroj tooa šablony.</span><span class="sxs-lookup"><span data-stu-id="e978c-104">Typically, you deploy all hello resources in your template tooa single resource group.</span></span> <span data-ttu-id="e978c-105">Existují však scénářích, kdy chcete toodeploy sadu prostředků společně, ale je umístit do jiné skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="e978c-105">However, there are scenarios where you want toodeploy a set of resources together but place them in different resource groups.</span></span> <span data-ttu-id="e978c-106">Například můžete toodeploy hello zálohování virtuálního počítače pro skupinu tooa samostatné prostředků Azure Site Recovery a umístění.</span><span class="sxs-lookup"><span data-stu-id="e978c-106">For example, you may want toodeploy hello backup virtual machine for Azure Site Recovery tooa separate resource group and location.</span></span> <span data-ttu-id="e978c-107">Resource Manager umožňuje toouse vnořené šablony tootarget různé skupiny prostředků než skupina prostředků hello používá hello nadřazené šablony.</span><span class="sxs-lookup"><span data-stu-id="e978c-107">Resource Manager enables you toouse nested templates tootarget different resource groups than hello resource group used for hello parent template.</span></span>

<span data-ttu-id="e978c-108">Skupina prostředků Hello je kontejner hello životního cyklu aplikace hello a jeho kolekce prostředků.</span><span class="sxs-lookup"><span data-stu-id="e978c-108">hello resource group is hello lifecycle container for hello application and its collection of resources.</span></span> <span data-ttu-id="e978c-109">Vytvořte skupinu prostředků hello mimo hello šablony a zadat tootarget skupiny prostředků hello během nasazování.</span><span class="sxs-lookup"><span data-stu-id="e978c-109">You create hello resource group outside of hello template, and specify hello resource group tootarget during deployment.</span></span> <span data-ttu-id="e978c-110">Úvod tooresource skupiny, najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e978c-110">For an introduction tooresource groups, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

## <a name="example-template"></a><span data-ttu-id="e978c-111">Příklad šablony</span><span class="sxs-lookup"><span data-stu-id="e978c-111">Example template</span></span>

<span data-ttu-id="e978c-112">tootarget různých prostředků, musíte použít šablonu vnořené nebo propojené během nasazení.</span><span class="sxs-lookup"><span data-stu-id="e978c-112">tootarget a different resource, you must use a nested or linked template during deployment.</span></span> <span data-ttu-id="e978c-113">Hello `Microsoft.Resources/deployments` nabízí typ prostředku `resourceGroup` parametr, který umožňuje toospecify jiné skupině prostředků pro hello vnořené nasazení.</span><span class="sxs-lookup"><span data-stu-id="e978c-113">hello `Microsoft.Resources/deployments` resource type provides a `resourceGroup` parameter that enables you toospecify a different resource group for hello nested deployment.</span></span> <span data-ttu-id="e978c-114">Všechny skupiny zdrojů hello musí existovat před spuštěním nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="e978c-114">All hello resource groups must exist before running hello deployment.</span></span> <span data-ttu-id="e978c-115">Hello následující příklad nasadí dva účty úložiště – jeden ve skupině prostředků hello zadávají během nasazení a po jednom v skupinu prostředků s názvem `crossResourceGroupDeployment`:</span><span class="sxs-lookup"><span data-stu-id="e978c-115">hello following example deploys two storage accounts - one in hello resource group specified during deployment, and one in a resource group named `crossResourceGroupDeployment`:</span></span>

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

<span data-ttu-id="e978c-116">Pokud nastavíte `resourceGroup` toohello název skupiny prostředků, který ještě neexistuje, nasazení hello se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="e978c-116">If you set `resourceGroup` toohello name of a resource group that does not exist, hello deployment fails.</span></span> <span data-ttu-id="e978c-117">Pokud nezadáte hodnotu `resourceGroup`, správce prostředků používá skupinu prostředků nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="e978c-117">If you do not provide a value for `resourceGroup`, Resource Manager uses hello parent resource group.</span></span>  

## <a name="deploy-hello-template"></a><span data-ttu-id="e978c-118">Nasazení šablony hello</span><span class="sxs-lookup"><span data-stu-id="e978c-118">Deploy hello template</span></span>

<span data-ttu-id="e978c-119">toodeploy hello příklad šablony, můžete použít hello portálu, prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e978c-119">toodeploy hello example template, you can use hello portal, Azure PowerShell, or Azure CLI.</span></span> <span data-ttu-id="e978c-120">Pro prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure musíte použít verzi z může 2017 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e978c-120">For Azure PowerShell or Azure CLI, you must use a release from May 2017 or later.</span></span> <span data-ttu-id="e978c-121">Příklady Hello předpokládají hello šablona byla uložena místně jako soubor s názvem **crossrgdeployment.json**.</span><span class="sxs-lookup"><span data-stu-id="e978c-121">hello examples assume you have saved hello template locally as a file named **crossrgdeployment.json**.</span></span>

<span data-ttu-id="e978c-122">Pro prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="e978c-122">For PowerShell:</span></span>

```powershell
Login-AzureRmAccount

New-AzureRmResourceGroup -Name mainResourceGroup -Location "South Central US"
New-AzureRmResourceGroup -Name crossResourceGroupDeployment -Location "Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName mainResourceGroup `
  -TemplateFile c:\MyTemplates\crossrgdeployment.json
```

<span data-ttu-id="e978c-123">Pokud používáte Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="e978c-123">For Azure CLI:</span></span>

```azurecli
az login

az group create --name mainResourceGroup --location "South Central US"
az group create --name crossResourceGroupDeployment --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group mainResourceGroup \
    --template-file crossrgdeployment.json
```

<span data-ttu-id="e978c-124">Po dokončení nasazení uvidíte dvě skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="e978c-124">After deployment completes, you see two resource groups.</span></span> <span data-ttu-id="e978c-125">Každé skupině prostředků obsahuje účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e978c-125">Each resource group contains a storage account.</span></span>

## <a name="use-resourcegroup-function"></a><span data-ttu-id="e978c-126">Pomocí funkce resourceGroup()</span><span class="sxs-lookup"><span data-stu-id="e978c-126">Use resourceGroup() function</span></span>

<span data-ttu-id="e978c-127">Pro různé nasazení skupiny prostředků, hello [resouceGroup() funkce](resource-group-template-functions-resource.md#resourcegroup) vyřeší různě v závislosti na tom, jak je zadat vnořené šablony hello.</span><span class="sxs-lookup"><span data-stu-id="e978c-127">For cross resource group deployments, hello [resouceGroup() function](resource-group-template-functions-resource.md#resourcegroup) resolves differently based on how you specify hello nested template.</span></span> 

<span data-ttu-id="e978c-128">Pokud vložit jednu šablonu v rámci jiné šablony resouceGroup() v šabloně vnořené hello přeloží toohello nadřazené prostředků skupiny.</span><span class="sxs-lookup"><span data-stu-id="e978c-128">If you embed one template within another template, resouceGroup() in hello nested template resolves toohello parent resource group.</span></span> <span data-ttu-id="e978c-129">Šablonu embedded používá hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="e978c-129">An embedded template uses hello following format:</span></span>

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() refers tooparent resource group
    }
}
```

<span data-ttu-id="e978c-130">Pokud jste šablonu samostatné tooa, resouceGroup() v šabloně propojené hello přeloží toohello vnořeného prostředku skupiny.</span><span class="sxs-lookup"><span data-stu-id="e978c-130">If you link tooa separate template, resouceGroup() in hello linked template resolves toohello nested resource group.</span></span> <span data-ttu-id="e978c-131">Propojené šablona používá hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="e978c-131">A linked template uses hello following format:</span></span>

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() in linked template refers toolinked resource group
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="e978c-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e978c-132">Next steps</span></span>

* <span data-ttu-id="e978c-133">jak zjistit, toodefine parametry v šabloně, toounderstand [pochopit strukturu hello a syntaxe šablon Azure Resource Manager](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="e978c-133">toounderstand how toodefine parameters in your template, see [Understand hello structure and syntax of Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="e978c-134">Tipy k řešení běžných chyb při nasazení, naleznete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="e978c-134">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="e978c-135">Informace o nasazení šablony, která vyžaduje tokenu SAS naleznete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="e978c-135">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>
