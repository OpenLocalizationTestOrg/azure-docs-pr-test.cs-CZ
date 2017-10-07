---
title: "aaaDeploy prostředky tooAzure | Microsoft Docs"
description: "Pomocí Azure PowerShell nebo rozhraní příkazového řádku Azure tooAzure toodeploy prostředky. Hello prostředky jsou definovány v šabloně Resource Manager."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/16/2017
ms.author: tomfitz
ms.openlocfilehash: 0cd3f8ad45af1fb85c78899b56f6807d00b859f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-tooazure"></a>Nasazení tooAzure prostředky

Toto téma ukazuje, jak tooyour toodeploy prostředky předplatného Azure. Můžete vytvořit prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure toodeploy šablony Resource Manageru, která definuje hello infrastrukturu pro vaše řešení.

Tooconcepts Úvod Resource Manager, najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).

## <a name="steps-for-deployment"></a>Postup nasazení

Toto téma předpokládá, že nasazujete hello [příklad úložiště šablony](#example-storage-template) v tomto tématu. Můžete použít jinou šablonu, ale hello parametry, které předat jsou jiné než je uvedeno v tomto tématu.

Po vytvoření šablony, hello obecné kroky pro nasazení šablony jsou:

1. Přihlaste se tooyour účtu
2. Vyberte toouse předplatné hello (jenom nezbytné Pokud máte více předplatných a chcete toouse, ten, který není hello výchozí předplatné)
3. Vytvoření skupiny prostředků
4. Nasazení šablony hello
5. Kontrola stavu nasazení

Hello následující části vysvětlují, jak tooperform ty kroky s [prostředí PowerShell](#powershell) nebo [rozhraní příkazového řádku Azure](#azure-cli).

## <a name="powershell"></a>PowerShell

1. tooinstall prostředí Azure PowerShell najdete v části [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/overview).

2. tooquickly Začínáme s nasazováním, použijte hello následující rutiny:

  ```powershell
  Login-AzureRmAccount
  Set-AzureRmContext -SubscriptionID {subscription-id}

  New-AzureRmResourceGroup -Name ExampleGroup -Location "Central US"
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json 
  ```

  Hello `Set-AzureRmContext` rutiny je potřeba jenom, pokud chcete toouse předplatné než výchozí předplatné. toosee všechny odběry a jejich ID, použijte:

  ```powershell
  Get-AzureRmSubscription
  ```

3. Hello nasazení může trvat několik minut toocomplete. Po dokončení se zobrazí zpráva podobná této:

  ```powershell
  DeploymentName          : ExampleDeployment
  CorrelationId           : 07b3b233-8367-4a0b-b9bc-7aa189065665
  ResourceGroupName       : ExampleGroup
  ProvisioningState       : Succeeded
  ...
  ```

4. toosee, které byly účtu skupiny a úložiště prostředků nasadit tooyour předplatného, použijte:

  ```powershell
  Get-AzureRmResourceGroup -Name ExampleGroup
  Find-AzureRmResource -ResourceGroupNameEquals ExampleGroup
  ```

5. Při nasazování šablony můžete zadat parametry šablony jako parametry PowerShellu. Hello předchozího příkladu neobsahuje žádné parametry šablony, aby byly použity výchozí hodnoty hello v šabloně hello. toodeploy úložiště jiný účet a zadejte hodnoty parametrů pro předponu názvu hello úložiště a účet úložiště hello SKU, použijte:

  ```powershell
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment2 -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json -storageNamePrefix "contoso" -storageSKU "Standard_GRS"
  ```

  Nyní máte ve skupině prostředků dva účty úložiště. 

## <a name="azure-cli"></a>Azure CLI

1. tooinstall příkazového řádku Azure CLI, najdete v části [nainstalovat Azure CLI 2.0](/cli/azure/install-az-cli2).

2. tooquickly Začínáme s nasazováním, použijte hello následující příkazy:

  ```azurecli
  az login
  az account set --subscription {subscription-id}

  az group create --name ExampleGroup --location "Central US"
  az group deployment create --name ExampleDeployment --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json
  ```

  Hello `az account set` příkaz je nutný pouze v případě, že chcete toouse předplatné než výchozí předplatné. toosee všechny odběry a jejich ID, použijte:

  ```azurecli
  az account list
  ```

3. Hello nasazení může trvat několik minut toocomplete. Po dokončení se zobrazí zpráva podobná této:

  ```azurecli
  "provisioningState": "Succeeded",
  ```

4. toosee, které byly účtu skupiny a úložiště prostředků nasadit tooyour předplatného, použijte:

  ```azurecli
  az group show --name ExampleGroup
  az resource list --resource-group ExampleGroup
  ```

5. Při nasazování šablony můžete zadat parametry šablony jako parametry PowerShellu. Hello předchozího příkladu neobsahuje žádné parametry šablony, aby byly použity výchozí hodnoty hello v šabloně hello. toodeploy úložiště jiný účet a zadejte hodnoty parametrů pro předponu názvu hello úložiště a účet úložiště hello SKU, použijte:

  ```azurecli
  az group deployment create --name ExampleDeployment2 --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json --parameters '{"storageNamePrefix":{"value":"contoso"},"storageSKU":{"value":"Standard_GRS"}}'
  ```

  Nyní máte ve skupině prostředků dva účty úložiště. 

## <a name="example-storage-template"></a>Příklad šablony úložiště

Použijte následující příklad šablony toodeploy tooyour předplatné účtu úložiště hello:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "hello value toouse for starting hello storage account name."
      }
    },
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
    }
  },
  "variables": {
    "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="next-steps"></a>Další kroky

* Podrobné informace o používání prostředí PowerShell toodeploy šablony najdete v tématu [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](/azure/azure-resource-manager/resource-group-template-deploy).
* Podrobné informace o používání rozhraní příkazového řádku Azure toodeploy šablony najdete v tématu [nasazení prostředků pomocí šablony Resource Manageru a rozhraní příkazového řádku Azure](/azure/azure-resource-manager/resource-group-template-deploy-cli).



