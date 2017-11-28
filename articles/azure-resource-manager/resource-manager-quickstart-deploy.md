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
# <a name="deploy-resources-tooazure"></a><span data-ttu-id="40294-104">Nasazení tooAzure prostředky</span><span class="sxs-lookup"><span data-stu-id="40294-104">Deploy resources tooAzure</span></span>

<span data-ttu-id="40294-105">Toto téma ukazuje, jak tooyour toodeploy prostředky předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="40294-105">This topic shows how toodeploy resources tooyour Azure subscription.</span></span> <span data-ttu-id="40294-106">Můžete vytvořit prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure toodeploy šablony Resource Manageru, která definuje hello infrastrukturu pro vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="40294-106">You can use either Azure PowerShell or Azure CLI toodeploy a Resource Manager template that defines hello infrastructure for your solution.</span></span>

<span data-ttu-id="40294-107">Tooconcepts Úvod Resource Manager, najdete v části [přehled Azure Resource Manageru](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="40294-107">For an introduction tooconcepts of Resource Manager, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

## <a name="steps-for-deployment"></a><span data-ttu-id="40294-108">Postup nasazení</span><span class="sxs-lookup"><span data-stu-id="40294-108">Steps for deployment</span></span>

<span data-ttu-id="40294-109">Toto téma předpokládá, že nasazujete hello [příklad úložiště šablony](#example-storage-template) v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="40294-109">This topic assumes you are deploying hello [example storage template](#example-storage-template) in this topic.</span></span> <span data-ttu-id="40294-110">Můžete použít jinou šablonu, ale hello parametry, které předat jsou jiné než je uvedeno v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="40294-110">You can use a different template, but hello parameters you pass are different than what is shown in this topic.</span></span>

<span data-ttu-id="40294-111">Po vytvoření šablony, hello obecné kroky pro nasazení šablony jsou:</span><span class="sxs-lookup"><span data-stu-id="40294-111">After creating a template, hello general steps for deploying your template are:</span></span>

1. <span data-ttu-id="40294-112">Přihlaste se tooyour účtu</span><span class="sxs-lookup"><span data-stu-id="40294-112">Log in tooyour account</span></span>
2. <span data-ttu-id="40294-113">Vyberte toouse předplatné hello (jenom nezbytné Pokud máte více předplatných a chcete toouse, ten, který není hello výchozí předplatné)</span><span class="sxs-lookup"><span data-stu-id="40294-113">Select hello subscription toouse (only necessary if you have multiple subscriptions, and you want toouse one that is not hello default subscription)</span></span>
3. <span data-ttu-id="40294-114">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="40294-114">Create a resource group</span></span>
4. <span data-ttu-id="40294-115">Nasazení šablony hello</span><span class="sxs-lookup"><span data-stu-id="40294-115">Deploy hello template</span></span>
5. <span data-ttu-id="40294-116">Kontrola stavu nasazení</span><span class="sxs-lookup"><span data-stu-id="40294-116">Check your deployment status</span></span>

<span data-ttu-id="40294-117">Hello následující části vysvětlují, jak tooperform ty kroky s [prostředí PowerShell](#powershell) nebo [rozhraní příkazového řádku Azure](#azure-cli).</span><span class="sxs-lookup"><span data-stu-id="40294-117">hello following sections show how tooperform those steps with [PowerShell](#powershell) or [Azure CLI](#azure-cli).</span></span>

## <a name="powershell"></a><span data-ttu-id="40294-118">PowerShell</span><span class="sxs-lookup"><span data-stu-id="40294-118">PowerShell</span></span>

1. <span data-ttu-id="40294-119">tooinstall prostředí Azure PowerShell najdete v části [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="40294-119">tooinstall Azure PowerShell, see [Get started with Azure PowerShell cmdlets](/powershell/azure/overview).</span></span>

2. <span data-ttu-id="40294-120">tooquickly Začínáme s nasazováním, použijte hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="40294-120">tooquickly get started with deployment, use hello following cmdlets:</span></span>

  ```powershell
  Login-AzureRmAccount
  Set-AzureRmContext -SubscriptionID {subscription-id}

  New-AzureRmResourceGroup -Name ExampleGroup -Location "Central US"
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json 
  ```

  <span data-ttu-id="40294-121">Hello `Set-AzureRmContext` rutiny je potřeba jenom, pokud chcete toouse předplatné než výchozí předplatné.</span><span class="sxs-lookup"><span data-stu-id="40294-121">hello `Set-AzureRmContext` cmdlet is only needed if you want toouse a subscription other than your default subscription.</span></span> <span data-ttu-id="40294-122">toosee všechny odběry a jejich ID, použijte:</span><span class="sxs-lookup"><span data-stu-id="40294-122">toosee all your subscriptions and their IDs, use:</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

3. <span data-ttu-id="40294-123">Hello nasazení může trvat několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="40294-123">hello deployment can take a few minutes toocomplete.</span></span> <span data-ttu-id="40294-124">Po dokončení se zobrazí zpráva podobná této:</span><span class="sxs-lookup"><span data-stu-id="40294-124">When it finishes, you see a message similar to:</span></span>

  ```powershell
  DeploymentName          : ExampleDeployment
  CorrelationId           : 07b3b233-8367-4a0b-b9bc-7aa189065665
  ResourceGroupName       : ExampleGroup
  ProvisioningState       : Succeeded
  ...
  ```

4. <span data-ttu-id="40294-125">toosee, které byly účtu skupiny a úložiště prostředků nasadit tooyour předplatného, použijte:</span><span class="sxs-lookup"><span data-stu-id="40294-125">toosee that your resource group and storage account were deployed tooyour subscription, use:</span></span>

  ```powershell
  Get-AzureRmResourceGroup -Name ExampleGroup
  Find-AzureRmResource -ResourceGroupNameEquals ExampleGroup
  ```

5. <span data-ttu-id="40294-126">Při nasazování šablony můžete zadat parametry šablony jako parametry PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="40294-126">You can specify template parameters as PowerShell parameters when deploying a template.</span></span> <span data-ttu-id="40294-127">Hello předchozího příkladu neobsahuje žádné parametry šablony, aby byly použity výchozí hodnoty hello v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="40294-127">hello earlier example did not include any template parameters, so hello default values in hello template were used.</span></span> <span data-ttu-id="40294-128">toodeploy úložiště jiný účet a zadejte hodnoty parametrů pro předponu názvu hello úložiště a účet úložiště hello SKU, použijte:</span><span class="sxs-lookup"><span data-stu-id="40294-128">toodeploy another storage account, and provide parameter values for hello storage name prefix and hello storage account SKU, use:</span></span>

  ```powershell
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment2 -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json -storageNamePrefix "contoso" -storageSKU "Standard_GRS"
  ```

  <span data-ttu-id="40294-129">Nyní máte ve skupině prostředků dva účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="40294-129">You now have two storage accounts in your resource group.</span></span> 

## <a name="azure-cli"></a><span data-ttu-id="40294-130">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="40294-130">Azure CLI</span></span>

1. <span data-ttu-id="40294-131">tooinstall příkazového řádku Azure CLI, najdete v části [nainstalovat Azure CLI 2.0](/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="40294-131">tooinstall Azure CLI, see [Install Azure CLI 2.0](/cli/azure/install-az-cli2).</span></span>

2. <span data-ttu-id="40294-132">tooquickly Začínáme s nasazováním, použijte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="40294-132">tooquickly get started with deployment, use hello following commands:</span></span>

  ```azurecli
  az login
  az account set --subscription {subscription-id}

  az group create --name ExampleGroup --location "Central US"
  az group deployment create --name ExampleDeployment --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json
  ```

  <span data-ttu-id="40294-133">Hello `az account set` příkaz je nutný pouze v případě, že chcete toouse předplatné než výchozí předplatné.</span><span class="sxs-lookup"><span data-stu-id="40294-133">hello `az account set` command is only needed if you want toouse a subscription other than your default subscription.</span></span> <span data-ttu-id="40294-134">toosee všechny odběry a jejich ID, použijte:</span><span class="sxs-lookup"><span data-stu-id="40294-134">toosee all your subscriptions and their IDs, use:</span></span>

  ```azurecli
  az account list
  ```

3. <span data-ttu-id="40294-135">Hello nasazení může trvat několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="40294-135">hello deployment can take a few minutes toocomplete.</span></span> <span data-ttu-id="40294-136">Po dokončení se zobrazí zpráva podobná této:</span><span class="sxs-lookup"><span data-stu-id="40294-136">When it finishes, you see a message similar to:</span></span>

  ```azurecli
  "provisioningState": "Succeeded",
  ```

4. <span data-ttu-id="40294-137">toosee, které byly účtu skupiny a úložiště prostředků nasadit tooyour předplatného, použijte:</span><span class="sxs-lookup"><span data-stu-id="40294-137">toosee that your resource group and storage account were deployed tooyour subscription, use:</span></span>

  ```azurecli
  az group show --name ExampleGroup
  az resource list --resource-group ExampleGroup
  ```

5. <span data-ttu-id="40294-138">Při nasazování šablony můžete zadat parametry šablony jako parametry PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="40294-138">You can specify template parameters as PowerShell parameters when deploying a template.</span></span> <span data-ttu-id="40294-139">Hello předchozího příkladu neobsahuje žádné parametry šablony, aby byly použity výchozí hodnoty hello v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="40294-139">hello earlier example did not include any template parameters, so hello default values in hello template were used.</span></span> <span data-ttu-id="40294-140">toodeploy úložiště jiný účet a zadejte hodnoty parametrů pro předponu názvu hello úložiště a účet úložiště hello SKU, použijte:</span><span class="sxs-lookup"><span data-stu-id="40294-140">toodeploy another storage account, and provide parameter values for hello storage name prefix and hello storage account SKU, use:</span></span>

  ```azurecli
  az group deployment create --name ExampleDeployment2 --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json --parameters '{"storageNamePrefix":{"value":"contoso"},"storageSKU":{"value":"Standard_GRS"}}'
  ```

  <span data-ttu-id="40294-141">Nyní máte ve skupině prostředků dva účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="40294-141">You now have two storage accounts in your resource group.</span></span> 

## <a name="example-storage-template"></a><span data-ttu-id="40294-142">Příklad šablony úložiště</span><span class="sxs-lookup"><span data-stu-id="40294-142">Example storage template</span></span>

<span data-ttu-id="40294-143">Použijte následující příklad šablony toodeploy tooyour předplatné účtu úložiště hello:</span><span class="sxs-lookup"><span data-stu-id="40294-143">Use hello following example template toodeploy a storage account tooyour subscription:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="40294-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="40294-144">Next steps</span></span>

* <span data-ttu-id="40294-145">Podrobné informace o používání prostředí PowerShell toodeploy šablony najdete v tématu [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="40294-145">For detailed information about using PowerShell toodeploy templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](/azure/azure-resource-manager/resource-group-template-deploy).</span></span>
* <span data-ttu-id="40294-146">Podrobné informace o používání rozhraní příkazového řádku Azure toodeploy šablony najdete v tématu [nasazení prostředků pomocí šablony Resource Manageru a rozhraní příkazového řádku Azure](/azure/azure-resource-manager/resource-group-template-deploy-cli).</span><span class="sxs-lookup"><span data-stu-id="40294-146">For detailed information about using Azure CLI toodeploy templates, see [Deploy resources with Resource Manager templates and Azure CLI](/azure/azure-resource-manager/resource-group-template-deploy-cli).</span></span>



