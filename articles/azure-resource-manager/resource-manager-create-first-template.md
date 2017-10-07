---
title: "aaaCreate první šablony Azure Resource Manageru | Microsoft Docs"
description: "Podrobný průvodce toocreating první šablony Azure Resource Manager. Se dozvíte, jak toouse hello odkaz na šablonu pro šablonu hello toocreate účet úložiště."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 07/27/2017
ms.topic: get-started-article
ms.author: tomfitz
ms.openlocfilehash: 92e6d6bb7094fe0e4537ee080704967862804bdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-your-first-azure-resource-manager-template"></a>Vytvoření a nasazení první šablony Azure Resource Manageru
Toto téma vás provede kroky hello k vytvoření vaší první šablony Azure Resource Manageru. Šablony Resource Manageru jsou soubory JSON, které definují hello prostředky, které potřebujete toodeploy pro vaše řešení. Koncepty hello toounderstand přidružené k nasazení a správě řešení Azure najdete v části [přehled Azure Resource Manageru](resource-group-overview.md). Pokud máte existující prostředky a chcete tooget šablonu pro tyto prostředky, přečtěte si téma [Export šablony Azure Resource Manageru ze stávajících prostředků](resource-manager-export-template.md).

toocreate a opravit šablony, je nutné JSON editor. [Visual Studio Code](https://code.visualstudio.com/) je jednoduchý, opensourcový editor kódu pro různé platformy. Důrazně doporučujeme k vytváření šablon Resource Manageru používat Visual Studio Code. Toto téma předpokládá, že používáte VS Code, pokud však máte jiný editor JSON (například sadu Visual Studio), můžete použít ten.

## <a name="prerequisites"></a>Požadavky

* Visual Studio Code. V případě potřeby ho nainstalujte z adresy [https://code.visualstudio.com/](https://code.visualstudio.com/).
* Předplatné Azure. Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="create-template"></a>Vytvoření šablony

Začněme jednoduché šablonu, která nasadí tooyour předplatné účtu úložiště.

1. Vyberte **Soubor** > **Nový soubor**. 

   ![Nový soubor](./media/resource-manager-create-first-template/new-file.png)

2. Zkopírujte a vložte následující syntaxe JSON do souboru hello:

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
     },
     "variables": {
     },
     "resources": [
       {
         "name": "[concat('storage', uniqueString(resourceGroup().id))]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "sku": {
           "name": "Standard_LRS"
         },
         "kind": "Storage",
         "location": "South Central US",
         "tags": {},
         "properties": {}
       }
     ],
     "outputs": {  }
   }
   ```

   Názvy účtů úložiště mají několik omezení, díky kterým tooset obtížná. Název Hello musí být v rozmezí 3 až 24 znaků délku, použijte pouze čísla a malá písmena a musí být jedinečná. Hello předchozí šablona používá hello [uniqueString](resource-group-template-functions-string.md#uniquestring) funkce toogenerate hodnotu hash. toogive tato hodnota hash hodnoty více znamená, přidá hello předponu *úložiště*. 

3. Uložte tento soubor jako **azuredeploy.json** tooa místní složky.

   ![Uložení šablony](./media/resource-manager-create-first-template/save-template.png)

## <a name="deploy-template"></a>Nasazení šablony

Můžete je připraven toodeploy této šablony. Můžete použít PowerShell nebo rozhraní příkazového řádku Azure toocreate skupinu prostředků. Potom nasadíte skupinu prostředků toothat účet úložiště.

* Pro prostředí PowerShell použijte následující příkazy z hello složku obsahující hello šablony hello:

   ```powershell
   Login-AzureRmAccount
   
   New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
   New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json
   ```

* Pro místní instalaci rozhraní příkazového řádku Azure použijte následující příkazy z hello složku obsahující hello šablony hello:

   ```azurecli
   az login

   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file azuredeploy.json
   ```

Po dokončení nasazení účtu úložiště existuje ve skupině prostředků hello.

## <a name="deploy-template-from-cloud-shell"></a>Nasazení šablony ze služby Cloud Shell

Můžete použít [cloudové prostředí](../cloud-shell/overview.md) toorun hello rozhraní příkazového řádku Azure příkazy pro nasazení šablony. Ale je nutné nejdřív načíst šablony do hello sdílené složky pro vaše cloudové prostředí. Pokud jste ještě službu Cloud Shell nepoužívali, přečtěte si téma [Přehled služby Azure Cloud Shell](../cloud-shell/overview.md), kde najdete informace o jejím nastavení.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).   

2. Vyberte vaši skupinu prostředků služby Cloud Shell. vzor názvů Hello je `cloud-shell-storage-<region>`.

   ![Výběr skupiny prostředků](./media/resource-manager-create-first-template/select-cs-resource-group.png)

3. Vyberte účet úložiště hello pro své cloudové prostředí.

   ![Výběr účtu úložiště](./media/resource-manager-create-first-template/select-storage.png)

4. Vyberte **Soubory**.

   ![Výběr souborů](./media/resource-manager-create-first-template/select-files.png)

5. Vyberte hello sdílené složky pro cloudové prostředí. vzor názvů Hello je `cs-<user>-<domain>-com-<uniqueGuid>`.

   ![Výběr sdílené složky](./media/resource-manager-create-first-template/select-file-share.png)

6. Vyberte **Přidat adresář**.

   ![Přidání adresáře](./media/resource-manager-create-first-template/select-add-directory.png)

7. Pojmenujte ho **templates** a vyberte **OK**.

   ![Pojmenování adresáře](./media/resource-manager-create-first-template/name-templates.png)

8. Vyberte nový adresář.

   ![Výběr adresáře](./media/resource-manager-create-first-template/select-templates.png)

9. Vyberte **Nahrát**.

   ![Výběr nahrání](./media/resource-manager-create-first-template/select-upload.png)

10. Vyhledejte a nahrajte vaši šablonu.

   ![Nahrání souboru](./media/resource-manager-create-first-template/upload-files.png)

11. Otevřete hello řádku.

   ![Otevření služby Cloud Shell](./media/resource-manager-create-first-template/start-cloud-shell.png)

12. Zadejte následující příkazy v prostředí cloudu hello hello:

   ```azurecli
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json
   ```

Po dokončení nasazení účtu úložiště existuje ve skupině prostředků hello.

## <a name="customize-hello-template"></a>Přizpůsobení šablony hello

Šablona Hello funguje bez problémů, ale není flexibilní. Nasadí vždy místně redundantního úložiště tooSouth střed USA. Název Hello je vždy *úložiště* následuje hodnotu hash. tooenable pomocí hello šablony pro různé scénáře, přidání šablony toohello parametry.

Hello následující příklad ukazuje hello parametry části s dva parametry. první parametr Hello `storageSKU` vám umožní toospecify hello typ redundance. Omezuje hello hodnoty, které lze předat v toovalues, které jsou platné pro účet úložiště. Také určuje výchozí hodnotu. druhý parametr Hello `storageNamePrefix` je sada tooallow delší než 11 znaků. Určuje výchozí hodnotu.

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "hello type of replication toouse for hello storage account."
    }
  },
  "storageNamePrefix": {
    "type": "string",
    "maxLength": 11,
    "defaultValue": "storage",
    "metadata": {
      "description": "hello value toouse for starting hello storage account name. Use only lowercase letters and numbers."
    }
  }
},
```

V části proměnných hello přidat proměnné s názvem `storageName`. Spojuje v sobě hello předpona hodnoty z parametrů hello a hodnotu hash z hello [uniqueString](resource-group-template-functions-string.md#uniquestring) funkce. Používá hello [toLower](resource-group-template-functions-string.md#tolower) funkce tooconvert toolowercase všechny znaky.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

toouse tyto nové hodnoty pro váš účet úložiště, změňte definici prostředků hello:

```json
"resources": [
  {
    "name": "[variables('storageName')]",
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2016-01-01",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    "kind": "Storage",
    "location": "[resourceGroup().location]",
    "tags": {},
    "properties": {}
  }
],
```

Všimněte si, že hello název účtu úložiště hello je teď nastavená toohello proměnnou, kterou jste přidali. Název SKU Hello se nastaví hodnota toohello parametru hello. Hello umístění se nastaví hello stejné umístění jako skupina prostředků hello.

Uložte soubor. 

Po dokončení kroků hello v tomto článku, vaše šablona teď vypadá takto:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "hello type of replication toouse for hello storage account."
      }
    },   
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "hello value toouse for starting hello storage account name. Use only lowercase letters and numbers."
      }
    }
  },
  "variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {}
    }
  ],
  "outputs": {  }
}
```

## <a name="redeploy-template"></a>Opětovné nasazení šablony

Znovu nasaďte šablonu hello s různými hodnotami.

Pokud používáte PowerShell, použijte:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json -storageNamePrefix newstore -storageSKU Standard_RAGRS
```

Pokud používáte Azure CLI, použijte:

```azurecli
az group deployment create --resource-group examplegroup --template-file azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

Hello cloudové prostředí nahrajte změněné šablony toohello sdílené složky. Přepište stávající soubor hello. Pak použijte hello následující příkaz:

```azurecli
az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete, vyčistěte hello prostředky, které jste nasadili odstraněním skupiny prostředků hello.

Pokud používáte PowerShell, použijte:

```powershell
Remove-AzureRmResourceGroup -Name examplegroup
```

Pokud používáte Azure CLI, použijte:

```azurecli
az group delete --name examplegroup
```

## <a name="next-steps"></a>Další kroky
* toolearn Další informace o struktuře hello šablon, najdete v části [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).
* toolearn o hello vlastnosti pro účet úložiště, najdete v části [odkaz na šablonu účtů úložiště](/azure/templates/microsoft.storage/storageaccounts).
* tooview dokončení šablon pro mnoho různých typů řešení, najdete v části hello [šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/).
