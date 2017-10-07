---
title: "aaaDeploy prostředků pomocí Azure CLI a šablony | Microsoft Docs"
description: "Pomocí Azure Resource Manageru a rozhraní příkazového řádku Azure toodeploy tooAzure prostředky. Hello prostředky jsou definovány v šabloně Resource Manager."
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
ms.openlocfilehash: 9f8bb9a8720399390a407030d2d32bcd97d32f13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Nasazení prostředků pomocí šablon Resource Manageru a Azure CLI

Toto téma vysvětluje, jak toouse 2.0 rozhraní příkazového řádku Azure s toodeploy šablony Resource Manageru tooAzure vaše prostředky. Pokud nejste obeznámeni s hello koncepty nasazení a Správa řešení Azure najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).  

šablony Resource Manageru Hello nasadíte, může to být místní soubor na počítači, nebo externí soubor, který je umístěný v úložišti, jako je Githubu. Hello šablona nasazení v tomto článku je k dispozici v hello [Ukázka šablony](#sample-template) oddílu, nebo jako [Šablona účtu úložiště na webu GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

Pokud nemáte nainstalované rozhraní příkazového řádku Azure, můžete použít hello [cloudové prostředí](#deploy-template-from-cloud-shell).

## <a name="deploy-local-template"></a>Nasazení místního šablony

Při nasazování tooAzure prostředků, můžete:

1. Přihlaste se tooyour účet Azure
2. Vytvořte skupinu prostředků, která slouží jako kontejner hello hello nasazené prostředky. Hello název hello skupiny prostředků může obsahovat jenom alfanumerické znaky, tečky, podtržítka, pomlčky a závorky. Může být až too90 znaků. Nemůže končit tečkou.
3. Nasazení toohello prostředků skupiny hello šablony, který definuje prostředky toocreate hello

Šablona může obsahovat parametry, které umožňují toocustomize hello nasazení. Například můžete zadat hodnoty, které jsou přizpůsobené pro konkrétní prostředí (například vývoj, testování a provozním). Ukázka šablony Hello definuje parametr pro účet úložiště hello SKU. 

Hello následující ukázka vytvoří skupinu prostředků a nasadí šablonu z místního počítače:

```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

Hello nasazení může trvat několik minut toocomplete. Po dokončení zobrazí zprávu, která zahrnuje hello výsledek:

```azurecli
"provisioningState": "Succeeded",
```

## <a name="deploy-external-template"></a>Nasazení externí šablony

Místo uložení šablony Resource Manageru na místním počítači, dáte možná přednost toostore je v externí umístění. Šablony můžete uložit ve úložiště řízení zdrojů (například Githubu). Nebo byste je uložit v účtu úložiště Azure pro přístup ke sdílenému ve vaší organizaci.

toodeploy šablonu externí použít hello **šablony uri** parametr. Použijte hello URI v hello příklad toodeploy hello Ukázka šablony z Githubu.
   
```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
    --parameters storageAccountType=Standard_GRS
```

Hello předchozí příklad vyžaduje veřejně přístupná identifikátor URI pro hello šablony, která funguje pro většinu scénářů, protože vaše šablona by neměla zahrnovat citlivá data. Pokud potřebujete toospecify citlivá data (např. heslo správce), předejte jako parametr zabezpečené tuto hodnotu. Ale pokud nechcete, aby vaše šablona toobe veřejně přístupný, můžete chránit ho ukládáním do kontejner privátní úložiště. Informace o nasazení šablony, která vyžaduje token sdílený přístupový podpis (SAS) najdete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-cli-sas-token.md).

## <a name="deploy-template-from-cloud-shell"></a>Nasazení šablony ze služby Cloud Shell

Můžete použít [cloudové prostředí](../cloud-shell/overview.md) toorun hello rozhraní příkazového řádku Azure příkazy pro nasazení šablony. Ale je nutné nejdřív načíst šablony do hello sdílené složky pro vaše cloudové prostředí. Pokud jste ještě službu Cloud Shell nepoužívali, přečtěte si téma [Přehled služby Azure Cloud Shell](../cloud-shell/overview.md), kde najdete informace o jejím nastavení.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).   

2. Vyberte vaši skupinu prostředků služby Cloud Shell. vzor názvů Hello je `cloud-shell-storage-<region>`.

   ![Výběr skupiny prostředků](./media/resource-group-template-deploy-cli/select-cs-resource-group.png)

3. Vyberte účet úložiště hello pro své cloudové prostředí.

   ![Výběr účtu úložiště](./media/resource-group-template-deploy-cli/select-storage.png)

4. Vyberte **Soubory**.

   ![Výběr souborů](./media/resource-group-template-deploy-cli/select-files.png)

5. Vyberte hello sdílené složky pro cloudové prostředí. vzor názvů Hello je `cs-<user>-<domain>-com-<uniqueGuid>`.

   ![Výběr sdílené složky](./media/resource-group-template-deploy-cli/select-file-share.png)

6. Vyberte **Přidat adresář**.

   ![Přidání adresáře](./media/resource-group-template-deploy-cli/select-add-directory.png)

7. Pojmenujte ho **templates** a vyberte **OK**.

   ![Pojmenování adresáře](./media/resource-group-template-deploy-cli/name-templates.png)

8. Vyberte nový adresář.

   ![Výběr adresáře](./media/resource-group-template-deploy-cli/select-templates.png)

9. Vyberte **Nahrát**.

   ![Výběr nahrání](./media/resource-group-template-deploy-cli/select-upload.png)

10. Vyhledejte a nahrajte vaši šablonu.

   ![Nahrání souboru](./media/resource-group-template-deploy-cli/upload-files.png)

11. Otevřete hello řádku.

   ![Otevření služby Cloud Shell](./media/resource-group-template-deploy-cli/start-cloud-shell.png)

12. Zadejte následující příkazy v prostředí cloudu hello hello:

   ```azurecli
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageAccountType=Standard_GRS
   ```

## <a name="parameter-files"></a>Soubory parametrů

Místo předávání parametrů jako vložené hodnoty ve vašem skriptu, možná bude snazší toouse soubor JSON, který obsahuje hodnoty parametrů hello. soubor parametrů Hello musí být ve formátu hello:

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

Všimněte si, že část parametry hello obsahuje název parametru, který odpovídá hello parametr definovaný v šabloně (storageAccountType). Hello soubor parametrů obsahuje hodnotu pro parametr hello. Tato hodnota je předán toohello šablony automaticky během nasazení. Můžete vytvořit několik souborů parametr pro různé scénáře nasazení a pak předejte soubor hello odpovídající parametr. 

Zkopírujte hello předcházející příklad a uložte ho jako soubor s názvem `storage.parameters.json`.

použít toopass soubor místní parametru `@` toospecify místní soubor s názvem storage.parameters.json.

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

## <a name="test-a-template-deployment"></a>Testovací nasazení šablony

pomocí šablony a parametr hodnoty bez ve skutečnosti nasazení všechny prostředky tootest [ověření nasazení skupiny az](/cli/azure/group/deployment#validate). 

```azurecli
az group deployment validate \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

Pokud nejsou zjištěny žádné chyby, hello příkaz vrátí informace o hello testovací nasazení. Konkrétně, Všimněte si, že hello **chyba** hodnota je null.

```azurecli
{
  "error": null,
  "properties": {
      ...
```

Pokud dojde k chybě, hello příkaz vrátí chybovou zprávu. Například pokus toopass nesprávná hodnota pro účet úložiště hello SKU, vrátí hello následující chybě:

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'hello provided value 'badSKU' for hello template parameter 
      'storageAccountType' at line '13' and column '20' is not valid. hello parameter value is not part of hello allowed 
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

Pokud vaše šablona obsahuje chybu syntaxe, příkaz hello chybovou zprávu oznamující, že ho nebylo možné rozložit hello šablony. uvítací zprávu označuje číslo řádku hello a pozice hello při analýze.

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

režim dokončení toouse, použijte hello `mode` parametr:

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --mode Complete \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

## <a name="sample-template"></a>Ukázka šablony

Hello následující šablona se používá pro hello příklady v tomto tématu. Zkopírujte a uložte ho jako soubor s názvem storage.json. toounderstand jak toocreate této šablony najdete v části [vytvoření vaší první šablony Azure Resource Manager](resource-manager-create-first-template.md).  

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
* Hello příklady v tomto článku nasazení skupiny prostředků tooa prostředky ve vašem předplatném výchozí. toouse jiného předplatného, najdete v části [spravovat víc předplatných Azure](/cli/azure/manage-azure-subscriptions-azure-cli).
* Pro dokončení ukázkový skript, který nasadí šablonu, najdete v části [skript nasazení šablony Resource Manageru](resource-manager-samples-cli-deploy.md).
* jak zjistit, toodefine parametry v šabloně, toounderstand [pochopit strukturu hello a syntaxe šablon Azure Resource Manager](resource-group-authoring-templates.md).
* Tipy k řešení běžných chyb při nasazení, naleznete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).
* Informace o nasazení šablony, která vyžaduje tokenu SAS naleznete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-cli-sas-token.md).
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).
