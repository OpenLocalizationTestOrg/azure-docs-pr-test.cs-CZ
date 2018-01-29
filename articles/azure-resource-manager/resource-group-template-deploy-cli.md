---
title: "Nasazení prostředků pomocí Azure CLI a šablony | Microsoft Docs"
description: "Nasazení prostředky do Azure pomocí Azure Resource Manageru a rozhraní příkazového řádku Azure. Prostředky jsou definovány v šabloně Resource Manageru."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 493b7932-8d1e-4499-912c-26098282ec95
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: tomfitz
ms.openlocfilehash: 13154e41ebd4867de9af74340a69446400814f5a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Nasazení prostředků pomocí šablon Resource Manageru a Azure CLI

Toto téma vysvětluje, jak pomocí Azure CLI 2.0 šablony Resource Manageru k nasazení vašich prostředků Azure. Pokud nejste obeznámeni s koncepty nasazení a Správa řešení Azure najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).  

Šablony Resource Manageru, který nasazujete, může to být místní soubor na počítači, nebo externí soubor, který je umístěný v úložišti, jako je Githubu. Šablona nasazení v tomto článku je k dispozici v [vzorové šablony](#sample-template) oddílu, nebo jako [Šablona účtu úložiště na webu GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

Pokud nemáte nainstalované rozhraní příkazového řádku Azure, můžete použít [cloudové prostředí](#deploy-template-from-cloud-shell).

## <a name="deploy-local-template"></a>Nasazení místního šablony

Při nasazování prostředků do Azure, můžete:

1. Přihlaste se k účtu Azure
2. Vytvořte skupinu prostředků, která slouží jako kontejner pro nasazené prostředky. Název skupiny prostředků může obsahovat pouze alfanumerické znaky, tečky, podtržítka, pomlčky a závorky. Může být až 90 znaků. Nemůže končit tečkou.
3. Nasazení do skupiny prostředků definující zdrojů pro vytvoření šablony

Šablonu může obsahovat parametry, které vám umožní přizpůsobit nasazení. Například můžete zadat hodnoty, které jsou přizpůsobené pro konkrétní prostředí (například vývoj, testování a provozním). Ukázka šablony definuje parametr pro účet úložiště SKU. 

Následující příklad vytvoří skupinu prostředků a nasadí šablonu z místního počítače:

```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

Dokončení nasazení může trvat několik minut. Po dokončení zobrazí zprávu, která obsahuje výsledek:

```azurecli
"provisioningState": "Succeeded",
```

## <a name="deploy-external-template"></a>Nasazení externí šablony

Místo uložení šablony Resource Manageru na místním počítači, dáváte přednost uložit je do externího umístění. Šablony můžete uložit ve úložiště řízení zdrojů (například Githubu). Nebo byste je uložit v účtu úložiště Azure pro přístup ke sdílenému ve vaší organizaci.

Chcete-li nasadit externí šablonu, použijte **šablony uri** parametr. Identifikátor URI v příkladu použijte k nasazení ukázkové šablony z Githubu.
   
```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
    --parameters storageAccountType=Standard_GRS
```

V předchozím příkladu vyžaduje veřejně přístupná identifikátor URI pro šablony, která funguje pro většinu scénářů, protože vaše šablona by neměla zahrnovat citlivá data. Pokud budete muset zadat citlivá data (např. heslo správce), předejte jako parametr zabezpečené tuto hodnotu. Ale pokud nechcete, aby vaše šablona veřejně přístupný, můžete chránit jeho uložením v kontejneru privátní úložiště. Informace o nasazení šablony, která vyžaduje token sdílený přístupový podpis (SAS) najdete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-cli-sas-token.md).

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../includes/resource-manager-cloud-shell-deploy.md)]

V prostředí cloudu použijte následující příkazy:

   ```azurecli-interactive
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageAccountType=Standard_GRS
   ```

## <a name="parameter-files"></a>Soubory parametrů

Místo předávání parametrů jako vložené hodnoty ve vašem skriptu, možná bude jednodušší použít soubor JSON, který obsahuje hodnoty parametru. Soubor parametrů musí být v následujícím formátu:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageAccountType": {
         "value": "Standard_GRS"
     }
  }
}
```

Všimněte si, že sekci parametrů obsahuje název parametru, který odpovídá parametru definované v šabloně (storageAccountType). Soubor parametrů obsahuje hodnotu pro parametr. Tato hodnota se automaticky předán do šablony během nasazení. Můžete vytvořit několik souborů parametr pro různé scénáře nasazení a pak předejte soubor odpovídající parametr. 

V předchozím příkladu zkopírujte a uložte ho jako soubor s názvem `storage.parameters.json`.

Chcete-li předat soubor místní parametrů, použijte `@` zadat místní soubor s názvem storage.parameters.json.

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

## <a name="test-a-template-deployment"></a>Testovací nasazení šablony

K otestování šablony a parametr hodnoty bez ve skutečnosti nasazení všechny prostředky, použijte [ověření nasazení skupiny az](/cli/azure/group/deployment#validate). 

```azurecli
az group deployment validate \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

Pokud nejsou zjištěny žádné chyby, příkaz vrátí informace o testovací nasazení. Konkrétně, Všimněte si, že **chyba** hodnota je null.

```azurecli
{
  "error": null,
  "properties": {
      ...
```

Pokud dojde k chybě, příkaz vrátí chybovou zprávu. Probíhá pokus o předání nesprávná hodnota pro účet úložiště SKU, například vrátí následující chybu:

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'The provided value 'badSKU' for the template parameter 
      'storageAccountType' at line '13' and column '20' is not valid. The parameter value is not part of the allowed 
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

Pokud vaše šablona obsahuje chybu syntaxe, příkaz vrátí chybu oznamující, že ho nebylo možné rozložit šablony. Zpráva označuje číslo řádku a pozice chybu analýzy.

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template parse failed: 'After parsing a value an unexpected character was encountered:
      \". Path 'variables', line 31, position 3.'.",
    "target": null
  },
  "properties": null
}
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

Pokud chcete použít dokončení režimu, použijte `mode` parametr:

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --mode Complete \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

## <a name="sample-template"></a>Ukázka šablony

Příklady v tomto tématu slouží následující šablony. Zkopírujte a uložte ho jako soubor s názvem storage.json. Chcete-li pochopit, jak k vytvoření této šablony, přečtěte si téma [vytvoření vaší první šablony Azure Resource Manager](resource-manager-create-first-template.md).  

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
    }
  ],
  "outputs": {
      "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
      }
  }
}
```

## <a name="next-steps"></a>Další kroky
* V příkladech v tomto článku nasadit do skupiny prostředků v rámci vašeho předplatného výchozí prostředky. Použijte jiný odběr, najdete v tématu [spravovat víc předplatných Azure](/cli/azure/manage-azure-subscriptions-azure-cli).
* Pro dokončení ukázkový skript, který nasadí šablonu, najdete v části [skript nasazení šablony Resource Manageru](resource-manager-samples-cli-deploy.md).
* Chcete-li pochopit, jak definovat parametry v šabloně, přečtěte si téma [pochopit strukturu a syntaxe šablon Azure Resource Manager](resource-group-authoring-templates.md).
* Tipy k řešení běžných chyb při nasazení, naleznete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).
* Informace o nasazení šablony, která vyžaduje tokenu SAS naleznete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-cli-sas-token.md).
* Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).
