---
title: "Umístění prostředků Azure v šabloně | Microsoft Docs"
description: "Ukazuje, jak nastavit umístění pro prostředek v šablonu Azure Resource Manager"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: tomfitz
ms.openlocfilehash: 73e50a593c41e841dcaf184abb895406ff5001e9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="set-resource-location-in-azure-resource-manager-templates"></a><span data-ttu-id="34709-103">Nastavte umístění prostředku v šablonách Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="34709-103">Set resource location in Azure Resource Manager templates</span></span>
<span data-ttu-id="34709-104">Při nasazování šablony, je nutné zadat umístění každého prostředku.</span><span class="sxs-lookup"><span data-stu-id="34709-104">When deploying a template, you must provide a location for each resource.</span></span> <span data-ttu-id="34709-105">Toto téma ukazuje, jak k určení umístění, které jsou k dispozici pro vaše předplatné pro každý typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="34709-105">This topic shows how to determine the locations that are available to your subscription for each resource type.</span></span>

## <a name="determine-supported-locations"></a><span data-ttu-id="34709-106">Určení podporovaných umístění</span><span class="sxs-lookup"><span data-stu-id="34709-106">Determine supported locations</span></span>

<span data-ttu-id="34709-107">Úplný seznam podporovaných umístění pro každý typ prostředku, najdete v části [produkty podle oblasti](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="34709-107">For a complete list of supported locations for each resource type, see [Products available by region](https://azure.microsoft.com/regions/services/).</span></span> <span data-ttu-id="34709-108">Vaše předplatné však nemusí mít přístup do všech umístění v tomto seznamu.</span><span class="sxs-lookup"><span data-stu-id="34709-108">However, your subscription might not have access to all the locations in that list.</span></span> <span data-ttu-id="34709-109">Pokud chcete zobrazit seznam umístění, které jsou k dispozici pro vaše předplatné přizpůsobené, použijte prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="34709-109">To see a customized list of locations that are available to your subscription, use Azure PowerShell or Azure CLI.</span></span> 

<span data-ttu-id="34709-110">Následující příklad používá prostředí PowerShell a získat umístění pro `Microsoft.Web\sites` typ prostředku:</span><span class="sxs-lookup"><span data-stu-id="34709-110">The following example uses PowerShell to get the locations for the `Microsoft.Web\sites` resource type:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

<span data-ttu-id="34709-111">Následující příklad používá Azure CLI 2.0 získat umístění `Microsoft.Web\sites` typ prostředku:</span><span class="sxs-lookup"><span data-stu-id="34709-111">The following example uses Azure CLI 2.0 to get the locations for the `Microsoft.Web\sites` resource type:</span></span>

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="set-location-in-template"></a><span data-ttu-id="34709-112">Nastavení umístění v šabloně</span><span class="sxs-lookup"><span data-stu-id="34709-112">Set location in template</span></span>

<span data-ttu-id="34709-113">Po určení podporovaná umístění pro vaše prostředky, musíte nastavit toto umístění ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="34709-113">After determining the supported locations for your resources, you need to set that location in your template.</span></span> <span data-ttu-id="34709-114">Nejjednodušší způsob, jak nastavit tuto hodnotu je vytvoření skupiny prostředků v místě, které podporuje tyto typy prostředků a nastavit každý umístění na `[resourceGroup().location]`.</span><span class="sxs-lookup"><span data-stu-id="34709-114">The easiest way to set this value is to create a resource group in a location that supports the resource types, and set each location to `[resourceGroup().location]`.</span></span> <span data-ttu-id="34709-115">Můžete znovu nasaďte šablonu, kterou chcete skupiny prostředků v různých umístěních a nezmění žádné hodnoty v šabloně nebo parametry.</span><span class="sxs-lookup"><span data-stu-id="34709-115">You can redeploy the template to resource groups in different locations, and not change any values in the template or parameters.</span></span> 

<span data-ttu-id="34709-116">Následující příklad ukazuje, účet úložiště, který je nasazen do stejného umístění jako pro skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="34709-116">The following example shows a storage account that is deployed to the same location as the resource group:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

<span data-ttu-id="34709-117">Pokud potřebujete používat pevné kódování umístění v šabloně, zadejte název jednoho z podporovaných oblastí.</span><span class="sxs-lookup"><span data-stu-id="34709-117">If you need to hardcode the location in your template, provide the name of one of the supported regions.</span></span> <span data-ttu-id="34709-118">Následující příklad ukazuje, účet úložiště, který je vždy nasazený do Sever střední USA:</span><span class="sxs-lookup"><span data-stu-id="34709-118">The following example shows a storage account that is always deployed to North Central US:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storageloc', uniqueString(resourceGroup().id))]",
      "location": "North Central US",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

## <a name="next-steps"></a><span data-ttu-id="34709-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="34709-119">Next steps</span></span>
* <span data-ttu-id="34709-120">Doporučení, jak vytvořit šablony, najdete v části [osvědčené postupy pro vytváření šablon Azure Resource Manager](resource-manager-template-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="34709-120">For recommendations about how to create templates, see [Best practices for creating Azure Resource Manager templates](resource-manager-template-best-practices.md).</span></span>

