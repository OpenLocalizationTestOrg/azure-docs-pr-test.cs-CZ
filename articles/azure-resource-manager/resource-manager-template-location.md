---
title: "umístění prostředků aaaAzure v šabloně | Microsoft Docs"
description: "Ukazuje, jak tooset umístění pro prostředek v šablonu Azure Resource Manager"
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
ms.openlocfilehash: f2ad6ca6ac5f34484a2e5e57dd8d67c77dacc41a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-resource-location-in-azure-resource-manager-templates"></a><span data-ttu-id="c842e-103">Nastavte umístění prostředku v šablonách Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c842e-103">Set resource location in Azure Resource Manager templates</span></span>
<span data-ttu-id="c842e-104">Při nasazování šablony, je nutné zadat umístění každého prostředku.</span><span class="sxs-lookup"><span data-stu-id="c842e-104">When deploying a template, you must provide a location for each resource.</span></span> <span data-ttu-id="c842e-105">Toto téma ukazuje, jak zadejte toodetermine hello umístění, které jsou k dispozici tooyour předplatné pro každého prostředku.</span><span class="sxs-lookup"><span data-stu-id="c842e-105">This topic shows how toodetermine hello locations that are available tooyour subscription for each resource type.</span></span>

## <a name="determine-supported-locations"></a><span data-ttu-id="c842e-106">Určení podporovaných umístění</span><span class="sxs-lookup"><span data-stu-id="c842e-106">Determine supported locations</span></span>

<span data-ttu-id="c842e-107">Úplný seznam podporovaných umístění pro každý typ prostředku, najdete v části [produkty podle oblasti](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="c842e-107">For a complete list of supported locations for each resource type, see [Products available by region](https://azure.microsoft.com/regions/services/).</span></span> <span data-ttu-id="c842e-108">Vaše předplatné však nemusí mít přístup tooall hello umístění v tomto seznamu.</span><span class="sxs-lookup"><span data-stu-id="c842e-108">However, your subscription might not have access tooall hello locations in that list.</span></span> <span data-ttu-id="c842e-109">toosee přizpůsobeného seznamu umístění, které jsou k dispozici tooyour předplatného, použijte prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="c842e-109">toosee a customized list of locations that are available tooyour subscription, use Azure PowerShell or Azure CLI.</span></span> 

<span data-ttu-id="c842e-110">Hello následující příklad používá prostředí PowerShell tooget hello umístění pro hello `Microsoft.Web\sites` typ prostředku:</span><span class="sxs-lookup"><span data-stu-id="c842e-110">hello following example uses PowerShell tooget hello locations for hello `Microsoft.Web\sites` resource type:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

<span data-ttu-id="c842e-111">Hello následující příklad používá Azure CLI 2.0 tooget hello umístění pro hello `Microsoft.Web\sites` typ prostředku:</span><span class="sxs-lookup"><span data-stu-id="c842e-111">hello following example uses Azure CLI 2.0 tooget hello locations for hello `Microsoft.Web\sites` resource type:</span></span>

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="set-location-in-template"></a><span data-ttu-id="c842e-112">Nastavení umístění v šabloně</span><span class="sxs-lookup"><span data-stu-id="c842e-112">Set location in template</span></span>

<span data-ttu-id="c842e-113">Po určení umístění hello podporované pro vaše prostředky, je třeba tooset tohoto umístění v šabloně.</span><span class="sxs-lookup"><span data-stu-id="c842e-113">After determining hello supported locations for your resources, you need tooset that location in your template.</span></span> <span data-ttu-id="c842e-114">Nejjednodušší způsob, jak tooset tato hodnota je toocreate prostředek skupiny v umístění, které podporuje typy prostředků hello Hello a nastavte každé umístění příliš`[resourceGroup().location]`.</span><span class="sxs-lookup"><span data-stu-id="c842e-114">hello easiest way tooset this value is toocreate a resource group in a location that supports hello resource types, and set each location too`[resourceGroup().location]`.</span></span> <span data-ttu-id="c842e-115">Můžete znovu nasadit hello šablony tooresource skupiny v různých umístěních a není změnit všechny hodnoty v šabloně hello nebo parametry.</span><span class="sxs-lookup"><span data-stu-id="c842e-115">You can redeploy hello template tooresource groups in different locations, and not change any values in hello template or parameters.</span></span> 

<span data-ttu-id="c842e-116">Hello následující příklad ukazuje, účet úložiště, který je nasazený toohello stejné umístění jako skupina prostředků hello:</span><span class="sxs-lookup"><span data-stu-id="c842e-116">hello following example shows a storage account that is deployed toohello same location as hello resource group:</span></span>

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

<span data-ttu-id="c842e-117">Pokud potřebujete toohardcode hello umístění v šabloně, zadejte název hello některé z oblastí hello podporována.</span><span class="sxs-lookup"><span data-stu-id="c842e-117">If you need toohardcode hello location in your template, provide hello name of one of hello supported regions.</span></span> <span data-ttu-id="c842e-118">Hello následující příklad ukazuje, že účet úložiště, který je pořád nasazené tooNorth střed USA:</span><span class="sxs-lookup"><span data-stu-id="c842e-118">hello following example shows a storage account that is always deployed tooNorth Central US:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="c842e-119">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c842e-119">Next steps</span></span>
* <span data-ttu-id="c842e-120">Pro doporučení, jak toocreate šablony, viz [osvědčené postupy pro vytváření šablon Azure Resource Manager](resource-manager-template-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="c842e-120">For recommendations about how toocreate templates, see [Best practices for creating Azure Resource Manager templates](resource-manager-template-best-practices.md).</span></span>

