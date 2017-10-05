---
title: "Export šablony Resource Manageru pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Export šablony ze skupiny prostředků pomocí Azure Resource Manager a rozhraní příkazového řádku Azure."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/01/2017
ms.author: tomfitz
ms.openlocfilehash: 617664129a5353e25da1e90c742c4b009db172ef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="export-azure-resource-manager-templates-with-azure-cli"></a><span data-ttu-id="06354-103">Export šablony Azure Resource Manager pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="06354-103">Export Azure Resource Manager templates with Azure CLI</span></span>

<span data-ttu-id="06354-104">Resource Manager vám umožňuje exportovat šablonu Resource Manageru z existujících prostředků ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="06354-104">Resource Manager enables you to export a Resource Manager template from existing resources in your subscription.</span></span> <span data-ttu-id="06354-105">Z vygenerované šablony pak zjistíte syntaxi šablony a podle potřeby pak můžete automatizovat opakované nasazení svého řešení.</span><span class="sxs-lookup"><span data-stu-id="06354-105">You can use that generated template to learn about the template syntax or to automate the redeployment of your solution as needed.</span></span>

<span data-ttu-id="06354-106">Je důležité si uvědomit, že jsou dva různé způsoby exportu šablony:</span><span class="sxs-lookup"><span data-stu-id="06354-106">It is important to note that there are two different ways to export a template:</span></span>

* <span data-ttu-id="06354-107">Je možné exportovat skutečnou šablonu, kterou jste použili k nasazení.</span><span class="sxs-lookup"><span data-stu-id="06354-107">You can export the actual template that you used for a deployment.</span></span> <span data-ttu-id="06354-108">Exportovaná šablona zahrnuje všechny parametry a proměnné přesně tak, jak jsou uvedeny v původní šabloně.</span><span class="sxs-lookup"><span data-stu-id="06354-108">The exported template includes all the parameters and variables exactly as they appeared in the original template.</span></span> <span data-ttu-id="06354-109">Tento přístup je užitečné, když potřebujete načíst šablonu.</span><span class="sxs-lookup"><span data-stu-id="06354-109">This approach is helpful when you need to retrieve a template.</span></span>
* <span data-ttu-id="06354-110">Můžete exportovat šablonu, která představuje aktuální stav skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="06354-110">You can export a template that represents the current state of the resource group.</span></span> <span data-ttu-id="06354-111">Exportovaná šablona není založena na žádné šabloně, kterou jste použili k nasazení.</span><span class="sxs-lookup"><span data-stu-id="06354-111">The exported template is not based on any template that you used for deployment.</span></span> <span data-ttu-id="06354-112">Místo toho export vytvoří šablonu, která je snímkem aktuálního stavu skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="06354-112">Instead, it creates a template that is a snapshot of the resource group.</span></span> <span data-ttu-id="06354-113">Exportovaná šablona má řadu pevně definovaných hodnot a pravděpodobně méně parametrů, než byste obvykle definovali.</span><span class="sxs-lookup"><span data-stu-id="06354-113">The exported template has many hard-coded values and probably not as many parameters as you would typically define.</span></span> <span data-ttu-id="06354-114">Tento přístup je užitečné, když jste změnili skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="06354-114">This approach is useful when you have modified the resource group.</span></span> <span data-ttu-id="06354-115">a teď potřebujete zachytit skupinu prostředků jako šablonu.</span><span class="sxs-lookup"><span data-stu-id="06354-115">Now, you need to capture the resource group as a template.</span></span>

<span data-ttu-id="06354-116">Toto téma ukazuje oba přístupy.</span><span class="sxs-lookup"><span data-stu-id="06354-116">This topic shows both approaches.</span></span>

## <a name="deploy-a-solution"></a><span data-ttu-id="06354-117">Nasazení řešení</span><span class="sxs-lookup"><span data-stu-id="06354-117">Deploy a solution</span></span>

<span data-ttu-id="06354-118">Pro ilustraci obou přístupů pro export šablony, Začněme tím, že nasazení řešení do vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="06354-118">To illustrate both approaches for exporting a template, let's start by deploying a solution to your subscription.</span></span> <span data-ttu-id="06354-119">Pokud už máte skupinu prostředků v rámci vašeho předplatného, který chcete exportovat, nemáte nasazení tohoto řešení.</span><span class="sxs-lookup"><span data-stu-id="06354-119">If you already have a resource group in your subscription that you want to export, you do not have to deploy this solution.</span></span> <span data-ttu-id="06354-120">Zbývající část tohoto článku, ale odkazuje na šablonu pro toto řešení.</span><span class="sxs-lookup"><span data-stu-id="06354-120">However, the remainder of this article refers to the template for this solution.</span></span> <span data-ttu-id="06354-121">Ukázkový skript nasadí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="06354-121">The example script deploys a storage account.</span></span>

```azurecli
az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name NewStorage \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
```  

## <a name="save-template-from-deployment-history"></a><span data-ttu-id="06354-122">Uložit šablonu z historie nasazení</span><span class="sxs-lookup"><span data-stu-id="06354-122">Save template from deployment history</span></span>

<span data-ttu-id="06354-123">Šablonu z historie nasazení můžete načíst pomocí [export nasazení skupiny az](/cli/azure/group/deployment#export) příkaz.</span><span class="sxs-lookup"><span data-stu-id="06354-123">You can retrieve a template from your deployment history by using the [az group deployment export](/cli/azure/group/deployment#export) command.</span></span> <span data-ttu-id="06354-124">Následující příklad uloží šablony, která dříve nasazení:</span><span class="sxs-lookup"><span data-stu-id="06354-124">The following example saves the template that you previously deploy:</span></span>

```azurecli
az group deployment export --name NewStorage --resource-group ExampleGroup
```

<span data-ttu-id="06354-125">Vrátí šablony.</span><span class="sxs-lookup"><span data-stu-id="06354-125">It returns the template.</span></span> <span data-ttu-id="06354-126">Zkopírujte JSON a uložte jako soubor.</span><span class="sxs-lookup"><span data-stu-id="06354-126">Copy the JSON, and save as a file.</span></span> <span data-ttu-id="06354-127">Všimněte si, že je přesný šablonu, kterou jste použili pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="06354-127">Notice that it is the exact template you used for deployment.</span></span> <span data-ttu-id="06354-128">Parametry a proměnné odpovídat šablony z Githubu.</span><span class="sxs-lookup"><span data-stu-id="06354-128">The parameters and variables match the template from GitHub.</span></span> <span data-ttu-id="06354-129">Tuto šablonu můžete znovu nasadit.</span><span class="sxs-lookup"><span data-stu-id="06354-129">You can redeploy this template.</span></span>


## <a name="export-resource-group-as-template"></a><span data-ttu-id="06354-130">Export skupiny prostředků jako šablony.</span><span class="sxs-lookup"><span data-stu-id="06354-130">Export resource group as template</span></span>

<span data-ttu-id="06354-131">Místo načítání šablonu z historie nasazení, můžete načíst šablonu, která představuje aktuální stav skupiny prostředků pomocí [export skupiny az](/cli/azure/group#export) příkaz.</span><span class="sxs-lookup"><span data-stu-id="06354-131">Instead of retrieving a template from the deployment history, you can retrieve a template that represents the current state of a resource group by using the [az group export](/cli/azure/group#export) command.</span></span> <span data-ttu-id="06354-132">Tento příkaz používají, když jste provedli mnoho změn vaší skupiny prostředků a žádné existující šablona představuje všechny změny.</span><span class="sxs-lookup"><span data-stu-id="06354-132">You use this command when you have made many changes to your resource group and no existing template represents all the changes.</span></span>

```azurecli
az group export --name ExampleGroup
```

<span data-ttu-id="06354-133">Vrátí šablony.</span><span class="sxs-lookup"><span data-stu-id="06354-133">It returns the template.</span></span> <span data-ttu-id="06354-134">Zkopírujte JSON a uložte jako soubor.</span><span class="sxs-lookup"><span data-stu-id="06354-134">Copy the JSON, and save as a file.</span></span> <span data-ttu-id="06354-135">Všimněte si, že se liší od šablony na webu GitHub.</span><span class="sxs-lookup"><span data-stu-id="06354-135">Notice that it is different than the template in GitHub.</span></span> <span data-ttu-id="06354-136">Obsahuje různé parametry a žádné proměnné.</span><span class="sxs-lookup"><span data-stu-id="06354-136">It has different parameters and no variables.</span></span> <span data-ttu-id="06354-137">Úložiště SKU a umístění jsou pevně zakódovaná na hodnoty.</span><span class="sxs-lookup"><span data-stu-id="06354-137">The storage SKU and location are hard-coded to values.</span></span> <span data-ttu-id="06354-138">Následující příklad ukazuje vyexportované šablony, ale vaše šablona má název parametru mírně odlišný:</span><span class="sxs-lookup"><span data-stu-id="06354-138">The following example shows the exported template, but your template has a slightly different parameter name:</span></span>

```json
{
  "parameters": {
    "storageAccounts_mcyzaljiv7qncstandardsa_name": {
      "type": "String",
      "defaultValue": null
    }
  },
  "variables": {},
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "resources": [
    {
      "tags": {},
      "dependsOn": [],
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "Storage",
      "type": "Microsoft.Storage/storageAccounts",
      "location": "centralus",
      "name": "[parameters('storageAccounts_mcyzaljiv7qncstandardsa_name')]",
      "properties": {}
    }
  ],
  "contentVersion": "1.0.0.0"
}
```

<span data-ttu-id="06354-139">Můžete znovu nasadit této šablony, ale vyžaduje to uhodnutí jedinečný název pro účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="06354-139">You can redeploy this template, but it requires guessing a unique name for the storage account.</span></span> <span data-ttu-id="06354-140">Název parametru se mírně liší.</span><span class="sxs-lookup"><span data-stu-id="06354-140">The name of your parameter is slightly different.</span></span>

```azurecli
az group deployment create --name NewStorage --resource-group ExampleGroup \
  --template-file examplegroup.json \
  --parameters "{\"storageAccounts_mcyzaljiv7qncstandardsa_name\":{\"value\":\"tfstore0501\"}}"
```

## <a name="customize-exported-template"></a><span data-ttu-id="06354-141">Přizpůsobení exportované šablony</span><span class="sxs-lookup"><span data-stu-id="06354-141">Customize exported template</span></span>

<span data-ttu-id="06354-142">Tato šablona, aby bylo snadné použití a flexibilnější, můžete upravit.</span><span class="sxs-lookup"><span data-stu-id="06354-142">You can modify this template to make it easier to use and more flexible.</span></span> <span data-ttu-id="06354-143">Povolit pro více umístění, změňte hodnotu vlastnosti umístění použít stejné umístění jako pro skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="06354-143">To allow for more locations, change the location property to use the same location as the resource group:</span></span>

```json
"location": "[resourceGroup().location]",
```

<span data-ttu-id="06354-144">Chcete-li předejít tak snadno uhodnout uniques název pro účet úložiště, odeberte parametr pro název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="06354-144">To avoid having to guess a uniques name for storage account, remove the parameter for the storage account name.</span></span> <span data-ttu-id="06354-145">Přidání parametru pro přípony názvu úložiště a úložiště SKU:</span><span class="sxs-lookup"><span data-stu-id="06354-145">Add a parameter for a storage name suffix, and a storage SKU:</span></span>

```json
"parameters": {
    "storageSuffix": {
      "type": "string",
      "defaultValue": "standardsa"
    },
    "storageAccountType": {
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS"
      ],
      "type": "string",
      "metadata": {
        "description": "Storage Account type"
      }
    }
},
```

<span data-ttu-id="06354-146">Přidání proměnné, která vytvoří název účtu úložiště pomocí funkce uniqueString:</span><span class="sxs-lookup"><span data-stu-id="06354-146">Add a variable that constructs the storage account name with the uniqueString function:</span></span>

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

<span data-ttu-id="06354-147">Název účtu úložiště nastavte proměnnou:</span><span class="sxs-lookup"><span data-stu-id="06354-147">Set the name of the storage account to the variable:</span></span>

```json
"name": "[variables('storageAccountName')]",
```

<span data-ttu-id="06354-148">Verze SKU nastaven na parametr:</span><span class="sxs-lookup"><span data-stu-id="06354-148">Set the SKU to the parameter:</span></span>

```json
"sku": {
    "name": "[parameters('storageAccountType')]",
    "tier": "Standard"
},
```

<span data-ttu-id="06354-149">Vaše šablona teď vypadá nějak takto:</span><span class="sxs-lookup"><span data-stu-id="06354-149">Your template now looks like:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageSuffix": {
      "type": "string",
      "defaultValue": "standardsa"
    },
    "storageAccountType": {
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS"
      ],
      "type": "string",
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), parameters('storageSuffix'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "sku": {
        "name": "[parameters('storageAccountType')]",
        "tier": "Standard"
      },
      "kind": "Storage",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {},
      "dependsOn": []
    }
  ]
}
```

<span data-ttu-id="06354-150">Znovu nasaďte změněné šablony.</span><span class="sxs-lookup"><span data-stu-id="06354-150">Redeploy the modified template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="06354-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="06354-151">Next steps</span></span>
* <span data-ttu-id="06354-152">Informace o používání portálu Export šablony najdete v tématu [Export šablony Azure Resource Manageru ze stávajících prostředků](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="06354-152">For information about using the portal to export a template, see [Export an Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>
* <span data-ttu-id="06354-153">Chcete-li definovat parametry v šabloně, přečtěte si téma [vytváření šablon](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="06354-153">To define parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="06354-154">Tipy k řešení běžných chyb při nasazení, naleznete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="06354-154">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>