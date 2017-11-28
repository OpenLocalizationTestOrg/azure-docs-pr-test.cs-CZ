---
title: "aaaExport šablony Resource Manageru pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Pomocí Azure Resource Manageru a rozhraní příkazového řádku Azure tooexport šablonu ze skupiny prostředků."
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
ms.openlocfilehash: 2d44a0a6e9717504d4c2a01254d826679b381f22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="export-azure-resource-manager-templates-with-azure-cli"></a><span data-ttu-id="21914-103">Export šablony Azure Resource Manager pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="21914-103">Export Azure Resource Manager templates with Azure CLI</span></span>

<span data-ttu-id="21914-104">Resource Manager umožňuje tooexport šablony Resource Manageru ze stávajících prostředků ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="21914-104">Resource Manager enables you tooexport a Resource Manager template from existing resources in your subscription.</span></span> <span data-ttu-id="21914-105">Můžete použít tento vygenerované šablony toolearn o hello šablony syntaxe nebo tooautomate hello opakované nasazení svého řešení podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="21914-105">You can use that generated template toolearn about hello template syntax or tooautomate hello redeployment of your solution as needed.</span></span>

<span data-ttu-id="21914-106">Je důležité toonote, že existují dva různé způsoby tooexport šablonu:</span><span class="sxs-lookup"><span data-stu-id="21914-106">It is important toonote that there are two different ways tooexport a template:</span></span>

* <span data-ttu-id="21914-107">Můžete exportovat hello skutečné šablonu, kterou jste použili pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="21914-107">You can export hello actual template that you used for a deployment.</span></span> <span data-ttu-id="21914-108">Hello vyexportované šablony zahrnuje všechny hello parametry a proměnné přesně tak, jak jsou uvedeny v původní šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="21914-108">hello exported template includes all hello parameters and variables exactly as they appeared in hello original template.</span></span> <span data-ttu-id="21914-109">Tento přístup je užitečné, když potřebujete tooretrieve šablonu.</span><span class="sxs-lookup"><span data-stu-id="21914-109">This approach is helpful when you need tooretrieve a template.</span></span>
* <span data-ttu-id="21914-110">Můžete exportovat šablonu, která představuje hello aktuální stav skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="21914-110">You can export a template that represents hello current state of hello resource group.</span></span> <span data-ttu-id="21914-111">Exportovaná šablona Hello není založena na všechny šablony, který jste použili pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="21914-111">hello exported template is not based on any template that you used for deployment.</span></span> <span data-ttu-id="21914-112">Místo toho vytvoří šablonu, která je snímek hello skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="21914-112">Instead, it creates a template that is a snapshot of hello resource group.</span></span> <span data-ttu-id="21914-113">Exportovaná šablona Hello má mnoho hodnot pevně a pravděpodobně ne tolik parametrů by obvykle definujete.</span><span class="sxs-lookup"><span data-stu-id="21914-113">hello exported template has many hard-coded values and probably not as many parameters as you would typically define.</span></span> <span data-ttu-id="21914-114">Tento přístup je užitečné, když jste změnili hello skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="21914-114">This approach is useful when you have modified hello resource group.</span></span> <span data-ttu-id="21914-115">Nyní musíte skupinu prostředků hello toocapture jako šablona.</span><span class="sxs-lookup"><span data-stu-id="21914-115">Now, you need toocapture hello resource group as a template.</span></span>

<span data-ttu-id="21914-116">Toto téma ukazuje oba přístupy.</span><span class="sxs-lookup"><span data-stu-id="21914-116">This topic shows both approaches.</span></span>

## <a name="deploy-a-solution"></a><span data-ttu-id="21914-117">Nasazení řešení</span><span class="sxs-lookup"><span data-stu-id="21914-117">Deploy a solution</span></span>

<span data-ttu-id="21914-118">tooillustrate obou blíží pro export šablony, Začněme tím, že nasazení řešení tooyour předplatné.</span><span class="sxs-lookup"><span data-stu-id="21914-118">tooillustrate both approaches for exporting a template, let's start by deploying a solution tooyour subscription.</span></span> <span data-ttu-id="21914-119">Pokud už máte skupinu prostředků v rámci vašeho předplatného, které chcete tooexport, není nutné toodeploy toto řešení.</span><span class="sxs-lookup"><span data-stu-id="21914-119">If you already have a resource group in your subscription that you want tooexport, you do not have toodeploy this solution.</span></span> <span data-ttu-id="21914-120">Hello zbývající část tohoto článku, ale odkazuje toohello šablonu pro toto řešení.</span><span class="sxs-lookup"><span data-stu-id="21914-120">However, hello remainder of this article refers toohello template for this solution.</span></span> <span data-ttu-id="21914-121">Ukázkový skript Hello nasadí účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="21914-121">hello example script deploys a storage account.</span></span>

```azurecli
az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name NewStorage \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
```  

## <a name="save-template-from-deployment-history"></a><span data-ttu-id="21914-122">Uložit šablonu z historie nasazení</span><span class="sxs-lookup"><span data-stu-id="21914-122">Save template from deployment history</span></span>

<span data-ttu-id="21914-123">Šablonu z historie nasazení můžete načíst pomocí hello [export nasazení skupiny az](/cli/azure/group/deployment#export) příkaz.</span><span class="sxs-lookup"><span data-stu-id="21914-123">You can retrieve a template from your deployment history by using hello [az group deployment export](/cli/azure/group/deployment#export) command.</span></span> <span data-ttu-id="21914-124">Následující příklad uloží hello šablonu, kterou jste dříve nasazení Hello:</span><span class="sxs-lookup"><span data-stu-id="21914-124">hello following example saves hello template that you previously deploy:</span></span>

```azurecli
az group deployment export --name NewStorage --resource-group ExampleGroup
```

<span data-ttu-id="21914-125">Vrátí hello šablony.</span><span class="sxs-lookup"><span data-stu-id="21914-125">It returns hello template.</span></span> <span data-ttu-id="21914-126">Zkopírujte hello JSON a uložte jako soubor.</span><span class="sxs-lookup"><span data-stu-id="21914-126">Copy hello JSON, and save as a file.</span></span> <span data-ttu-id="21914-127">Všimněte si, že se jedná o hello přesný šablonu, kterou jste použili pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="21914-127">Notice that it is hello exact template you used for deployment.</span></span> <span data-ttu-id="21914-128">Hello parametry a proměnné odpovídat hello šablony z Githubu.</span><span class="sxs-lookup"><span data-stu-id="21914-128">hello parameters and variables match hello template from GitHub.</span></span> <span data-ttu-id="21914-129">Tuto šablonu můžete znovu nasadit.</span><span class="sxs-lookup"><span data-stu-id="21914-129">You can redeploy this template.</span></span>


## <a name="export-resource-group-as-template"></a><span data-ttu-id="21914-130">Export skupiny prostředků jako šablony.</span><span class="sxs-lookup"><span data-stu-id="21914-130">Export resource group as template</span></span>

<span data-ttu-id="21914-131">Místo načítání šablonu z historie hello nasazení, můžete načíst šablonu, která představuje hello aktuální stav skupiny prostředků pomocí hello [export skupiny az](/cli/azure/group#export) příkaz.</span><span class="sxs-lookup"><span data-stu-id="21914-131">Instead of retrieving a template from hello deployment history, you can retrieve a template that represents hello current state of a resource group by using hello [az group export](/cli/azure/group#export) command.</span></span> <span data-ttu-id="21914-132">Tento příkaz používají, když jste provedli mnoho skupiny prostředků tooyour změny a žádné existující šablona představuje všechny změny hello.</span><span class="sxs-lookup"><span data-stu-id="21914-132">You use this command when you have made many changes tooyour resource group and no existing template represents all hello changes.</span></span>

```azurecli
az group export --name ExampleGroup
```

<span data-ttu-id="21914-133">Vrátí hello šablony.</span><span class="sxs-lookup"><span data-stu-id="21914-133">It returns hello template.</span></span> <span data-ttu-id="21914-134">Zkopírujte hello JSON a uložte jako soubor.</span><span class="sxs-lookup"><span data-stu-id="21914-134">Copy hello JSON, and save as a file.</span></span> <span data-ttu-id="21914-135">Všimněte si, že se liší od hello šablony na webu GitHub.</span><span class="sxs-lookup"><span data-stu-id="21914-135">Notice that it is different than hello template in GitHub.</span></span> <span data-ttu-id="21914-136">Obsahuje různé parametry a žádné proměnné.</span><span class="sxs-lookup"><span data-stu-id="21914-136">It has different parameters and no variables.</span></span> <span data-ttu-id="21914-137">úložiště Hello SKU a umístění jsou pevně toovalues.</span><span class="sxs-lookup"><span data-stu-id="21914-137">hello storage SKU and location are hard-coded toovalues.</span></span> <span data-ttu-id="21914-138">Hello následující příklad ukazuje hello vyexportované šablony, ale vaše šablona má název parametru mírně odlišný:</span><span class="sxs-lookup"><span data-stu-id="21914-138">hello following example shows hello exported template, but your template has a slightly different parameter name:</span></span>

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

<span data-ttu-id="21914-139">Můžete znovu nasadit této šablony, ale vyžaduje to uhodnutí jedinečný název pro účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="21914-139">You can redeploy this template, but it requires guessing a unique name for hello storage account.</span></span> <span data-ttu-id="21914-140">Hello název vaší parametru se mírně liší.</span><span class="sxs-lookup"><span data-stu-id="21914-140">hello name of your parameter is slightly different.</span></span>

```azurecli
az group deployment create --name NewStorage --resource-group ExampleGroup \
  --template-file examplegroup.json \
  --parameters "{\"storageAccounts_mcyzaljiv7qncstandardsa_name\":{\"value\":\"tfstore0501\"}}"
```

## <a name="customize-exported-template"></a><span data-ttu-id="21914-141">Přizpůsobení exportované šablony</span><span class="sxs-lookup"><span data-stu-id="21914-141">Customize exported template</span></span>

<span data-ttu-id="21914-142">Můžete upravit toomake tuto šablonu je snazší toouse a flexibilnější.</span><span class="sxs-lookup"><span data-stu-id="21914-142">You can modify this template toomake it easier toouse and more flexible.</span></span> <span data-ttu-id="21914-143">tooallow pro další umístění, změna hello umístění vlastnost toouse hello stejné umístění jako skupina prostředků hello:</span><span class="sxs-lookup"><span data-stu-id="21914-143">tooallow for more locations, change hello location property toouse hello same location as hello resource group:</span></span>

```json
"location": "[resourceGroup().location]",
```

<span data-ttu-id="21914-144">tooavoid s tooguess uniques název pro účet úložiště, odeberte hello parametr pro název účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="21914-144">tooavoid having tooguess a uniques name for storage account, remove hello parameter for hello storage account name.</span></span> <span data-ttu-id="21914-145">Přidání parametru pro přípony názvu úložiště a úložiště SKU:</span><span class="sxs-lookup"><span data-stu-id="21914-145">Add a parameter for a storage name suffix, and a storage SKU:</span></span>

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

<span data-ttu-id="21914-146">Přidání proměnné, která vytvoří hello název účtu úložiště pomocí funkce uniqueString hello:</span><span class="sxs-lookup"><span data-stu-id="21914-146">Add a variable that constructs hello storage account name with hello uniqueString function:</span></span>

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

<span data-ttu-id="21914-147">Nastavte název hello hello úložiště účet toohello proměnné:</span><span class="sxs-lookup"><span data-stu-id="21914-147">Set hello name of hello storage account toohello variable:</span></span>

```json
"name": "[variables('storageAccountName')]",
```

<span data-ttu-id="21914-148">Nastavte parametr toohello SKU hello:</span><span class="sxs-lookup"><span data-stu-id="21914-148">Set hello SKU toohello parameter:</span></span>

```json
"sku": {
    "name": "[parameters('storageAccountType')]",
    "tier": "Standard"
},
```

<span data-ttu-id="21914-149">Vaše šablona teď vypadá nějak takto:</span><span class="sxs-lookup"><span data-stu-id="21914-149">Your template now looks like:</span></span>

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

<span data-ttu-id="21914-150">Znovu nasaďte šablonu upravené hello.</span><span class="sxs-lookup"><span data-stu-id="21914-150">Redeploy hello modified template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="21914-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="21914-151">Next steps</span></span>
* <span data-ttu-id="21914-152">Informace o použití portálu tooexport hello šablony najdete v tématu [Export šablony Azure Resource Manageru ze stávajících prostředků](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="21914-152">For information about using hello portal tooexport a template, see [Export an Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>
* <span data-ttu-id="21914-153">toodefine parametry v šabloně, najdete v části [vytváření šablon](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="21914-153">toodefine parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="21914-154">Tipy k řešení běžných chyb při nasazení, naleznete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="21914-154">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>