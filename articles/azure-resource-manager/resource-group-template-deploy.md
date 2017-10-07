---
title: "aaaDeploy prostředků pomocí prostředí PowerShell a šablony | Microsoft Docs"
description: "Pomocí Azure Resource Manageru a prostředí Azure PowerShell toodeploy tooAzure prostředky. Hello prostředky jsou definovány v šabloně Resource Manager."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 55903f35-6c16-4c6d-bf52-dbf365605c3f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 41506811ba3c2ea5df6313db70978ade50f71161
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Nasazení prostředků pomocí šablon Resource Manageru a Azure PowerShellu

Toto téma vysvětluje, jak toouse prostředí Azure PowerShell s Resource Manager šablony toodeploy tooAzure vaše prostředky. Pokud nejste obeznámeni s hello koncepty nasazení a Správa řešení Azure najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).

šablony Resource Manageru Hello nasadíte, může to být místní soubor na počítači, nebo externí soubor, který je umístěný v úložišti, jako je Githubu. Šablona Hello nasazení v tomto článku je k dispozici v hello [vzorové šablony](#sample-template) oddílu, nebo jako [Šablona účtu úložiště na webu GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).

[!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install.md)]

<a id="deploy-local-template" />

## <a name="deploy-a-template-from-your-local-machine"></a>Nasazení šablony z místního počítače

Při nasazování tooAzure prostředků, můžete:

1. Přihlaste se tooyour účet Azure
2. Vytvořte skupinu prostředků, která slouží jako kontejner hello hello nasazené prostředky. Hello název hello skupiny prostředků může obsahovat jenom alfanumerické znaky, tečky, podtržítka, pomlčky a závorky. Může být až too90 znaků. Nemůže končit tečkou.
3. Nasazení toohello prostředků skupiny hello šablony, který definuje prostředky toocreate hello

Šablona může obsahovat parametry, které umožňují toocustomize hello nasazení. Například můžete zadat hodnoty, které jsou přizpůsobené pro konkrétní prostředí (například vývoj, testování a provozním). Ukázka šablony Hello definuje parametr pro účet úložiště hello SKU.

Hello následující ukázka vytvoří skupinu prostředků a nasadí šablonu z místního počítače:

```powershell
Login-AzureRmAccount
 
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

Hello nasazení může trvat několik minut toocomplete. Po dokončení zobrazí zprávu, která zahrnuje hello výsledek:

```powershell
ProvisioningState       : Succeeded
```

## <a name="deploy-a-template-from-an-external-source"></a>Nasazení šablony z externího zdroje

Místo uložení šablony Resource Manageru na místním počítači, dáte možná přednost toostore je v externí umístění. Šablony můžete uložit ve úložiště řízení zdrojů (například Githubu). Nebo byste je uložit v účtu úložiště Azure pro přístup ke sdílenému ve vaší organizaci.

toodeploy šablonu externí použít hello **TemplateUri** parametr. Použijte hello URI v hello příklad toodeploy hello Ukázka šablony z Githubu.

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -storageAccountType Standard_GRS
```

Hello předchozí příklad vyžaduje veřejně přístupná identifikátor URI pro hello šablony, která funguje pro většinu scénářů, protože vaše šablona by neměla zahrnovat citlivá data. Pokud potřebujete toospecify citlivá data (např. heslo správce), předejte jako parametr zabezpečené tuto hodnotu. Ale pokud nechcete, aby vaše šablona toobe veřejně přístupný, můžete chránit ho ukládáním do kontejner privátní úložiště. Informace o nasazení šablony, která vyžaduje token sdílený přístupový podpis (SAS) najdete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-powershell-sas-token.md).

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

toopass soubor místní parametr použít hello **TemplateParameterFile** parametr:

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

toopass soubor externí parametr, použijte hello **TemplateParameterUri** parametr:

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

Můžete použít vložené parametry a místní parametr souborů v hello stejné operace nasazení. Můžete například zadat některé hodnoty v souboru místní parametr hello a přidání dalších hodnot vloženého během nasazení. Když poskytujete hodnoty pro parametr v souboru místní parametr hello i vložené, hello vložené přednost hodnota.

Ale při použití souboru externí parametr nemůžete předat další hodnoty buď vložené nebo z místního souboru. Pokud zadáte soubor parametrů v hello **TemplateParameterUri** parametr, všechny vnořené parametry jsou ignorovány. Zadejte všechny hodnoty parametrů v souboru externích hello. Pokud vaše šablona obsahuje citlivé hodnotu, která nelze zahrnout do souboru parametrů hello, přidejte tuto hodnotu trezoru klíčů tooa nebo dynamicky poskytovat všechny vnořené hodnoty parametru.

Pokud vaše šablona obsahuje parametr s hello stejný název jako jeden z parametrů hello v hello příkaz prostředí PowerShell, prostředí PowerShell představuje parametr hello z šablony s operátory hello **FromTemplate**. Například parametr s názvem **ResourceGroupName** na šablony v konfliktu s hello **ResourceGroupName** parametr v hello [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment)rutiny. Jsou výzvami tooprovide hodnotu **ResourceGroupNameFromTemplate**. Obecně platí neměli byste toto nedorozuměním není pojmenováním parametry s hello stejný název jako parametry použité pro operace nasazení.

## <a name="test-a-template-deployment"></a>Testovací nasazení šablony

pomocí šablony a parametr hodnoty bez ve skutečnosti nasazení všechny prostředky tootest [Test-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment). 

```powershell
Test-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

Pokud nejsou zjištěny žádné chyby, dokončení příkazu hello bez odezvy. Pokud dojde k chybě, hello příkaz vrátí chybovou zprávu. Například pokus toopass nesprávná hodnota pro účet úložiště hello SKU, vrátí hello následující chybě:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType badSku

Code    : InvalidTemplate
Message : Deployment template validation failed: 'hello provided value 'badSku' for hello template parameter 'storageAccountType'
          at line '15' and column '24' is not valid. hello parameter value is not part of hello allowed value(s):
          'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.
Details :
```

Pokud vaše šablona obsahuje chybu syntaxe, příkaz hello chybovou zprávu oznamující, že ho nebylo možné rozložit hello šablony. uvítací zprávu označuje číslo řádku hello a pozice hello při analýze.

```powershell
Test-AzureRmResourceGroupDeployment : After parsing a value an unexpected character was encountered: 
  ". Path 'variables', line 31, position 3.
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

režim dokončení toouse, použijte hello `Mode` parametr:

```powershell
New-AzureRmResourceGroupDeployment -Mode Complete -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup -TemplateFile c:\MyTemplates\storage.json 
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
* Hello příklady v tomto článku nasazení skupiny prostředků tooa prostředky ve vašem předplatném výchozí. toouse jiného předplatného, najdete v části [spravovat víc předplatných Azure](/powershell/azure/manage-subscriptions-azureps).
* Pro dokončení ukázkový skript, který nasadí šablonu, najdete v části [skript nasazení šablony Resource Manageru](resource-manager-samples-powershell-deploy.md).
* jak zjistit, toodefine parametry v šabloně, toounderstand [pochopit strukturu hello a syntaxe šablon Azure Resource Manager](resource-group-authoring-templates.md).
* Tipy k řešení běžných chyb při nasazení, naleznete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).
* Informace o nasazení šablony, která vyžaduje tokenu SAS naleznete v tématu [privátní šablony nasazení s tokenem SAS](resource-manager-powershell-sas-token.md).
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).

