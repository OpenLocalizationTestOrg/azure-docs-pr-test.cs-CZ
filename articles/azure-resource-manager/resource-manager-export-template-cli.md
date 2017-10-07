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
# <a name="export-azure-resource-manager-templates-with-azure-cli"></a>Export šablony Azure Resource Manager pomocí rozhraní příkazového řádku Azure

Resource Manager umožňuje tooexport šablony Resource Manageru ze stávajících prostředků ve vašem předplatném. Můžete použít tento vygenerované šablony toolearn o hello šablony syntaxe nebo tooautomate hello opakované nasazení svého řešení podle potřeby.

Je důležité toonote, že existují dva různé způsoby tooexport šablonu:

* Můžete exportovat hello skutečné šablonu, kterou jste použili pro nasazení. Hello vyexportované šablony zahrnuje všechny hello parametry a proměnné přesně tak, jak jsou uvedeny v původní šabloně hello. Tento přístup je užitečné, když potřebujete tooretrieve šablonu.
* Můžete exportovat šablonu, která představuje hello aktuální stav skupiny prostředků hello. Exportovaná šablona Hello není založena na všechny šablony, který jste použili pro nasazení. Místo toho vytvoří šablonu, která je snímek hello skupinu prostředků. Exportovaná šablona Hello má mnoho hodnot pevně a pravděpodobně ne tolik parametrů by obvykle definujete. Tento přístup je užitečné, když jste změnili hello skupinu prostředků. Nyní musíte skupinu prostředků hello toocapture jako šablona.

Toto téma ukazuje oba přístupy.

## <a name="deploy-a-solution"></a>Nasazení řešení

tooillustrate obou blíží pro export šablony, Začněme tím, že nasazení řešení tooyour předplatné. Pokud už máte skupinu prostředků v rámci vašeho předplatného, které chcete tooexport, není nutné toodeploy toto řešení. Hello zbývající část tohoto článku, ale odkazuje toohello šablonu pro toto řešení. Ukázkový skript Hello nasadí účet úložiště.

```azurecli
az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name NewStorage \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
```  

## <a name="save-template-from-deployment-history"></a>Uložit šablonu z historie nasazení

Šablonu z historie nasazení můžete načíst pomocí hello [export nasazení skupiny az](/cli/azure/group/deployment#export) příkaz. Následující příklad uloží hello šablonu, kterou jste dříve nasazení Hello:

```azurecli
az group deployment export --name NewStorage --resource-group ExampleGroup
```

Vrátí hello šablony. Zkopírujte hello JSON a uložte jako soubor. Všimněte si, že se jedná o hello přesný šablonu, kterou jste použili pro nasazení. Hello parametry a proměnné odpovídat hello šablony z Githubu. Tuto šablonu můžete znovu nasadit.


## <a name="export-resource-group-as-template"></a>Export skupiny prostředků jako šablony.

Místo načítání šablonu z historie hello nasazení, můžete načíst šablonu, která představuje hello aktuální stav skupiny prostředků pomocí hello [export skupiny az](/cli/azure/group#export) příkaz. Tento příkaz používají, když jste provedli mnoho skupiny prostředků tooyour změny a žádné existující šablona představuje všechny změny hello.

```azurecli
az group export --name ExampleGroup
```

Vrátí hello šablony. Zkopírujte hello JSON a uložte jako soubor. Všimněte si, že se liší od hello šablony na webu GitHub. Obsahuje různé parametry a žádné proměnné. úložiště Hello SKU a umístění jsou pevně toovalues. Hello následující příklad ukazuje hello vyexportované šablony, ale vaše šablona má název parametru mírně odlišný:

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

Můžete znovu nasadit této šablony, ale vyžaduje to uhodnutí jedinečný název pro účet úložiště hello. Hello název vaší parametru se mírně liší.

```azurecli
az group deployment create --name NewStorage --resource-group ExampleGroup \
  --template-file examplegroup.json \
  --parameters "{\"storageAccounts_mcyzaljiv7qncstandardsa_name\":{\"value\":\"tfstore0501\"}}"
```

## <a name="customize-exported-template"></a>Přizpůsobení exportované šablony

Můžete upravit toomake tuto šablonu je snazší toouse a flexibilnější. tooallow pro další umístění, změna hello umístění vlastnost toouse hello stejné umístění jako skupina prostředků hello:

```json
"location": "[resourceGroup().location]",
```

tooavoid s tooguess uniques název pro účet úložiště, odeberte hello parametr pro název účtu úložiště hello. Přidání parametru pro přípony názvu úložiště a úložiště SKU:

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

Přidání proměnné, která vytvoří hello název účtu úložiště pomocí funkce uniqueString hello:

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

Nastavte název hello hello úložiště účet toohello proměnné:

```json
"name": "[variables('storageAccountName')]",
```

Nastavte parametr toohello SKU hello:

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

Znovu nasaďte šablonu upravené hello.

## <a name="next-steps"></a>Další kroky
* Informace o použití portálu tooexport hello šablony najdete v tématu [Export šablony Azure Resource Manageru ze stávajících prostředků](resource-manager-export-template.md).
* toodefine parametry v šabloně, najdete v části [vytváření šablon](resource-group-authoring-templates.md#parameters).
* Tipy k řešení běžných chyb při nasazení, naleznete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).