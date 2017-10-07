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
# <a name="set-resource-location-in-azure-resource-manager-templates"></a>Nastavte umístění prostředku v šablonách Azure Resource Manager
Při nasazování šablony, je nutné zadat umístění každého prostředku. Toto téma ukazuje, jak zadejte toodetermine hello umístění, které jsou k dispozici tooyour předplatné pro každého prostředku.

## <a name="determine-supported-locations"></a>Určení podporovaných umístění

Úplný seznam podporovaných umístění pro každý typ prostředku, najdete v části [produkty podle oblasti](https://azure.microsoft.com/regions/services/). Vaše předplatné však nemusí mít přístup tooall hello umístění v tomto seznamu. toosee přizpůsobeného seznamu umístění, které jsou k dispozici tooyour předplatného, použijte prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure. 

Hello následující příklad používá prostředí PowerShell tooget hello umístění pro hello `Microsoft.Web\sites` typ prostředku:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Hello následující příklad používá Azure CLI 2.0 tooget hello umístění pro hello `Microsoft.Web\sites` typ prostředku:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="set-location-in-template"></a>Nastavení umístění v šabloně

Po určení umístění hello podporované pro vaše prostředky, je třeba tooset tohoto umístění v šabloně. Nejjednodušší způsob, jak tooset tato hodnota je toocreate prostředek skupiny v umístění, které podporuje typy prostředků hello Hello a nastavte každé umístění příliš`[resourceGroup().location]`. Můžete znovu nasadit hello šablony tooresource skupiny v různých umístěních a není změnit všechny hodnoty v šabloně hello nebo parametry. 

Hello následující příklad ukazuje, účet úložiště, který je nasazený toohello stejné umístění jako skupina prostředků hello:

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

Pokud potřebujete toohardcode hello umístění v šabloně, zadejte název hello některé z oblastí hello podporována. Hello následující příklad ukazuje, že účet úložiště, který je pořád nasazené tooNorth střed USA:

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

## <a name="next-steps"></a>Další kroky
* Pro doporučení, jak toocreate šablony, viz [osvědčené postupy pro vytváření šablon Azure Resource Manager](resource-manager-template-best-practices.md).

