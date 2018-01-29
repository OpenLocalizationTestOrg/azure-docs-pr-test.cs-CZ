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
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="export-azure-resource-manager-templates-with-azure-cli"></a>Export šablony Azure Resource Manager pomocí rozhraní příkazového řádku Azure

Resource Manager vám umožňuje exportovat šablonu Resource Manageru z existujících prostředků ve vašem předplatném. Z vygenerované šablony pak zjistíte syntaxi šablony a podle potřeby pak můžete automatizovat opakované nasazení svého řešení.

Je důležité si uvědomit, že jsou dva různé způsoby exportu šablony:

* Je možné exportovat skutečnou šablonu, kterou jste použili k nasazení. Exportovaná šablona zahrnuje všechny parametry a proměnné přesně tak, jak jsou uvedeny v původní šabloně. Tento přístup je užitečné, když potřebujete načíst šablonu.
* Můžete exportovat šablonu, která představuje aktuální stav skupiny prostředků. Exportovaná šablona není založena na žádné šabloně, kterou jste použili k nasazení. Místo toho export vytvoří šablonu, která je snímkem aktuálního stavu skupiny prostředků. Exportovaná šablona má řadu pevně definovaných hodnot a pravděpodobně méně parametrů, než byste obvykle definovali. Tento přístup je užitečné, když jste změnili skupině prostředků. a teď potřebujete zachytit skupinu prostředků jako šablonu.

Toto téma ukazuje oba přístupy.

## <a name="deploy-a-solution"></a>Nasazení řešení

Pro ilustraci obou přístupů pro export šablony, Začněme tím, že nasazení řešení do vašeho předplatného. Pokud už máte skupinu prostředků v rámci vašeho předplatného, který chcete exportovat, nemáte nasazení tohoto řešení. Zbývající část tohoto článku, ale odkazuje na šablonu pro toto řešení. Ukázkový skript nasadí účet úložiště.

```azurecli
az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name NewStorage \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
```  

## <a name="save-template-from-deployment-history"></a>Uložit šablonu z historie nasazení

Šablonu z historie nasazení můžete načíst pomocí [export nasazení skupiny az](/cli/azure/group/deployment#export) příkaz. Následující příklad uloží šablony, která dříve nasazení:

```azurecli
az group deployment export --name NewStorage --resource-group ExampleGroup
```

Vrátí šablony. Zkopírujte JSON a uložte jako soubor. Všimněte si, že je přesný šablonu, kterou jste použili pro nasazení. Parametry a proměnné odpovídat šablony z Githubu. Tuto šablonu můžete znovu nasadit.


## <a name="export-resource-group-as-template"></a>Export skupiny prostředků jako šablony.

Místo načítání šablonu z historie nasazení, můžete načíst šablonu, která představuje aktuální stav skupiny prostředků pomocí [export skupiny az](/cli/azure/group#export) příkaz. Tento příkaz používají, když jste provedli mnoho změn vaší skupiny prostředků a žádné existující šablona představuje všechny změny.

```azurecli
az group export --name ExampleGroup
```

Vrátí šablony. Zkopírujte JSON a uložte jako soubor. Všimněte si, že se liší od šablony na webu GitHub. Obsahuje různé parametry a žádné proměnné. Úložiště SKU a umístění jsou pevně zakódovaná na hodnoty. Následující příklad ukazuje vyexportované šablony, ale vaše šablona má název parametru mírně odlišný:

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

Můžete znovu nasadit této šablony, ale vyžaduje to uhodnutí jedinečný název pro účet úložiště. Název parametru se mírně liší.

```azurecli
az group deployment create --name NewStorage --resource-group ExampleGroup \
  --template-file examplegroup.json \
  --parameters "{\"storageAccounts_mcyzaljiv7qncstandardsa_name\":{\"value\":\"tfstore0501\"}}"
```

## <a name="customize-exported-template"></a>Přizpůsobení exportované šablony

Tato šablona, aby bylo snadné použití a flexibilnější, můžete upravit. Povolit pro více umístění, změňte hodnotu vlastnosti umístění použít stejné umístění jako pro skupinu prostředků:

```json
"location": "[resourceGroup().location]",
```

Chcete-li předejít tak snadno uhodnout uniques název pro účet úložiště, odeberte parametr pro název účtu úložiště. Přidání parametru pro přípony názvu úložiště a úložiště SKU:

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

Přidání proměnné, která vytvoří název účtu úložiště pomocí funkce uniqueString:

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

Název účtu úložiště nastavte proměnnou:

```json
"name": "[variables('storageAccountName')]",
```

Verze SKU nastaven na parametr:

```json
"sku": {
    "name": "[parameters('storageAccountType')]",
    "tier": "Standard"
},
```

Vaše šablona teď vypadá nějak takto:

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

Znovu nasaďte změněné šablony.

## <a name="next-steps"></a>Další kroky
* Informace o používání portálu Export šablony najdete v tématu [Export šablony Azure Resource Manageru ze stávajících prostředků](resource-manager-export-template.md).
* Chcete-li definovat parametry v šabloně, přečtěte si téma [vytváření šablon](resource-group-authoring-templates.md#parameters).
* Tipy k řešení běžných chyb při nasazení, naleznete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).