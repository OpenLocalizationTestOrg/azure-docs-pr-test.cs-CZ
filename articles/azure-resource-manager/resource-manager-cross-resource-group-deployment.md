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
# <a name="deploy-azure-resources-toomore-than-one-resource-group"></a>Nasazení prostředků Azure toomore než jedné skupiny prostředků

Zpravidla nasazujete, všechny prostředky hello ve vaší skupině pro jediný zdroj tooa šablony. Existují však scénářích, kdy chcete toodeploy sadu prostředků společně, ale je umístit do jiné skupiny prostředků. Například můžete toodeploy hello zálohování virtuálního počítače pro skupinu tooa samostatné prostředků Azure Site Recovery a umístění. Resource Manager umožňuje toouse vnořené šablony tootarget různé skupiny prostředků než skupina prostředků hello používá hello nadřazené šablony.

Skupina prostředků Hello je kontejner hello životního cyklu aplikace hello a jeho kolekce prostředků. Vytvořte skupinu prostředků hello mimo hello šablony a zadat tootarget skupiny prostředků hello během nasazování. Úvod tooresource skupiny, najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).

## <a name="example-template"></a>Příklad šablony

tootarget různých prostředků, musíte použít šablonu vnořené nebo propojené během nasazení. Hello `Microsoft.Resources/deployments` nabízí typ prostředku `resourceGroup` parametr, který umožňuje toospecify jiné skupině prostředků pro hello vnořené nasazení. Všechny skupiny zdrojů hello musí existovat před spuštěním nasazení hello. Hello následující příklad nasadí dva účty úložiště – jeden ve skupině prostředků hello zadávají během nasazení a po jednom v skupinu prostředků s názvem `crossResourceGroupDeployment`:

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

Pokud nastavíte `resourceGroup` toohello název skupiny prostředků, který ještě neexistuje, nasazení hello se nezdaří. Pokud nezadáte hodnotu `resourceGroup`, správce prostředků používá skupinu prostředků nadřazené hello.  

## <a name="deploy-hello-template"></a>Nasazení šablony hello

toodeploy hello příklad šablony, můžete použít hello portálu, prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure. Pro prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure musíte použít verzi z může 2017 nebo novější. Příklady Hello předpokládají hello šablona byla uložena místně jako soubor s názvem **crossrgdeployment.json**.

Pro prostředí PowerShell:

```powershell
Login-AzureRmAccount

New-AzureRmResourceGroup -Name mainResourceGroup -Location "South Central US"
New-AzureRmResourceGroup -Name crossResourceGroupDeployment -Location "Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName mainResourceGroup `
  -TemplateFile c:\MyTemplates\crossrgdeployment.json
```

Pokud používáte Azure CLI:

```azurecli
az login

az group create --name mainResourceGroup --location "South Central US"
az group create --name crossResourceGroupDeployment --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group mainResourceGroup \
    --template-file crossrgdeployment.json
```

Po dokončení nasazení uvidíte dvě skupiny prostředků. Každé skupině prostředků obsahuje účet úložiště.

## <a name="use-resourcegroup-function"></a>Pomocí funkce resourceGroup()

Pro různé nasazení skupiny prostředků, hello [resouceGroup() funkce](resource-group-template-functions-resource.md#resourcegroup) vyřeší různě v závislosti na tom, jak je zadat vnořené šablony hello. 

Pokud vložit jednu šablonu v rámci jiné šablony resouceGroup() v šabloně vnořené hello přeloží toohello nadřazené prostředků skupiny. Šablonu embedded používá hello následující formát:

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

Pokud jste šablonu samostatné tooa, resouceGroup() v šabloně propojené hello přeloží toohello vnořeného prostředku skupiny. Propojené šablona používá hello následující formát:

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

## <a name="next-steps"></a>Další kroky

* jak zjistit, toodefine parametry v šabloně, toounderstand [pochopit strukturu hello a syntaxe šablon Azure Resource Manager](resource-group-authoring-templates.md).
* Tipy k řešení běžných chyb při nasazení, naleznete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).
* Informace o nasazení šablony, která vyžaduje tokenu SAS naleznete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-powershell-sas-token.md).
