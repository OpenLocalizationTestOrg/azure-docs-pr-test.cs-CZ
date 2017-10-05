---
title: "Nasazení prostředků do Azure | Dokumentace Microsoftu"
description: "Nasazení prostředků do Azure pomocí Azure PowerShellu nebo Azure CLI. Prostředky jsou definovány v šabloně Resource Manageru."
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
ms.openlocfilehash: 19d5ec337a18b1a159de05ed611b2ccd0c15c592
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-resources-to-azure"></a><span data-ttu-id="d3859-104">Nasazení prostředků do Azure</span><span class="sxs-lookup"><span data-stu-id="d3859-104">Deploy resources to Azure</span></span>

<span data-ttu-id="d3859-105">Toto téma ukazuje, jak nasadit prostředky do předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="d3859-105">This topic shows how to deploy resources to your Azure subscription.</span></span> <span data-ttu-id="d3859-106">K nasazení šablony Resource Manageru, která definuje infrastrukturu řešení, můžete použít Azure PowerShell nebo Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="d3859-106">You can use either Azure PowerShell or Azure CLI to deploy a Resource Manager template that defines the infrastructure for your solution.</span></span>

<span data-ttu-id="d3859-107">Úvod ke konceptům Resource Manageru najdete v tématu [Přehled Azure Resource Manageru](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d3859-107">For an introduction to concepts of Resource Manager, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

## <a name="steps-for-deployment"></a><span data-ttu-id="d3859-108">Postup nasazení</span><span class="sxs-lookup"><span data-stu-id="d3859-108">Steps for deployment</span></span>

<span data-ttu-id="d3859-109">Toto téma předpokládá, že nasazujete [ukázkovou šablonu úložiště](#example-storage-template) z tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="d3859-109">This topic assumes you are deploying the [example storage template](#example-storage-template) in this topic.</span></span> <span data-ttu-id="d3859-110">Můžete použít jinou šablonu, ale předávané parametry budou jiné než v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="d3859-110">You can use a different template, but the parameters you pass are different than what is shown in this topic.</span></span>

<span data-ttu-id="d3859-111">Po vytvoření šablony je obecný postup nasazení šablony následující:</span><span class="sxs-lookup"><span data-stu-id="d3859-111">After creating a template, the general steps for deploying your template are:</span></span>

1. <span data-ttu-id="d3859-112">Přihlášení k účtu</span><span class="sxs-lookup"><span data-stu-id="d3859-112">Log in to your account</span></span>
2. <span data-ttu-id="d3859-113">Výběr předplatného, které chcete použít (nutné pouze v případě, že máte více předplatných a chcete použít jiné než výchozí)</span><span class="sxs-lookup"><span data-stu-id="d3859-113">Select the subscription to use (only necessary if you have multiple subscriptions, and you want to use one that is not the default subscription)</span></span>
3. <span data-ttu-id="d3859-114">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="d3859-114">Create a resource group</span></span>
4. <span data-ttu-id="d3859-115">Nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="d3859-115">Deploy the template</span></span>
5. <span data-ttu-id="d3859-116">Kontrola stavu nasazení</span><span class="sxs-lookup"><span data-stu-id="d3859-116">Check your deployment status</span></span>

<span data-ttu-id="d3859-117">Následující části ukazují provedení těchto kroků pomocí [PowerShellu](#powershell) nebo [Azure CLI](#azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d3859-117">The following sections show how to perform those steps with [PowerShell](#powershell) or [Azure CLI](#azure-cli).</span></span>

## <a name="powershell"></a><span data-ttu-id="d3859-118">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d3859-118">PowerShell</span></span>

1. <span data-ttu-id="d3859-119">Informace o instalaci Azure PowerShellu najdete v tématu [Začínáme s rutinami Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d3859-119">To install Azure PowerShell, see [Get started with Azure PowerShell cmdlets](/powershell/azure/overview).</span></span>

2. <span data-ttu-id="d3859-120">Pokud chcete rychle začít s nasazením, použijte následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="d3859-120">To quickly get started with deployment, use the following cmdlets:</span></span>

  ```powershell
  Login-AzureRmAccount
  Set-AzureRmContext -SubscriptionID {subscription-id}

  New-AzureRmResourceGroup -Name ExampleGroup -Location "Central US"
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json 
  ```

  <span data-ttu-id="d3859-121">Rutina `Set-AzureRmContext` je potřeba pouze v případě, že chcete použít jiné předplatné, než je vaše výchozí.</span><span class="sxs-lookup"><span data-stu-id="d3859-121">The `Set-AzureRmContext` cmdlet is only needed if you want to use a subscription other than your default subscription.</span></span> <span data-ttu-id="d3859-122">Pokud chcete zobrazit všechna předplatná a jejich ID, použijte:</span><span class="sxs-lookup"><span data-stu-id="d3859-122">To see all your subscriptions and their IDs, use:</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

3. <span data-ttu-id="d3859-123">Dokončení nasazení může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="d3859-123">The deployment can take a few minutes to complete.</span></span> <span data-ttu-id="d3859-124">Po dokončení se zobrazí zpráva podobná této:</span><span class="sxs-lookup"><span data-stu-id="d3859-124">When it finishes, you see a message similar to:</span></span>

  ```powershell
  DeploymentName          : ExampleDeployment
  CorrelationId           : 07b3b233-8367-4a0b-b9bc-7aa189065665
  ResourceGroupName       : ExampleGroup
  ProvisioningState       : Succeeded
  ...
  ```

4. <span data-ttu-id="d3859-125">Pokud chcete ověřit nasazení skupiny prostředků a účtu úložiště do předplatného, použijte:</span><span class="sxs-lookup"><span data-stu-id="d3859-125">To see that your resource group and storage account were deployed to your subscription, use:</span></span>

  ```powershell
  Get-AzureRmResourceGroup -Name ExampleGroup
  Find-AzureRmResource -ResourceGroupNameEquals ExampleGroup
  ```

5. <span data-ttu-id="d3859-126">Při nasazování šablony můžete zadat parametry šablony jako parametry PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="d3859-126">You can specify template parameters as PowerShell parameters when deploying a template.</span></span> <span data-ttu-id="d3859-127">Předchozí příklad nezahrnoval žádné parametry šablony, takže se použily výchozí hodnoty z šablony.</span><span class="sxs-lookup"><span data-stu-id="d3859-127">The earlier example did not include any template parameters, so the default values in the template were used.</span></span> <span data-ttu-id="d3859-128">Pokud chcete nasadit další účet úložiště a zadat hodnoty parametrů pro předponu názvu úložiště a skladovou položku úložiště, použijte:</span><span class="sxs-lookup"><span data-stu-id="d3859-128">To deploy another storage account, and provide parameter values for the storage name prefix and the storage account SKU, use:</span></span>

  ```powershell
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment2 -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json -storageNamePrefix "contoso" -storageSKU "Standard_GRS"
  ```

  <span data-ttu-id="d3859-129">Nyní máte ve skupině prostředků dva účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="d3859-129">You now have two storage accounts in your resource group.</span></span> 

## <a name="azure-cli"></a><span data-ttu-id="d3859-130">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d3859-130">Azure CLI</span></span>

1. <span data-ttu-id="d3859-131">Informace o instalaci Azure CLI najdete v tématu [Instalace Azure CLI 2.0](/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="d3859-131">To install Azure CLI, see [Install Azure CLI 2.0](/cli/azure/install-az-cli2).</span></span>

2. <span data-ttu-id="d3859-132">Pokud chcete rychle začít s nasazením, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d3859-132">To quickly get started with deployment, use the following commands:</span></span>

  ```azurecli
  az login
  az account set --subscription {subscription-id}

  az group create --name ExampleGroup --location "Central US"
  az group deployment create --name ExampleDeployment --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json
  ```

  <span data-ttu-id="d3859-133">Příkaz `az account set` je potřeba pouze v případě, že chcete použít jiné předplatné, než je vaše výchozí.</span><span class="sxs-lookup"><span data-stu-id="d3859-133">The `az account set` command is only needed if you want to use a subscription other than your default subscription.</span></span> <span data-ttu-id="d3859-134">Pokud chcete zobrazit všechna předplatná a jejich ID, použijte:</span><span class="sxs-lookup"><span data-stu-id="d3859-134">To see all your subscriptions and their IDs, use:</span></span>

  ```azurecli
  az account list
  ```

3. <span data-ttu-id="d3859-135">Dokončení nasazení může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="d3859-135">The deployment can take a few minutes to complete.</span></span> <span data-ttu-id="d3859-136">Po dokončení se zobrazí zpráva podobná této:</span><span class="sxs-lookup"><span data-stu-id="d3859-136">When it finishes, you see a message similar to:</span></span>

  ```azurecli
  "provisioningState": "Succeeded",
  ```

4. <span data-ttu-id="d3859-137">Pokud chcete ověřit nasazení skupiny prostředků a účtu úložiště do předplatného, použijte:</span><span class="sxs-lookup"><span data-stu-id="d3859-137">To see that your resource group and storage account were deployed to your subscription, use:</span></span>

  ```azurecli
  az group show --name ExampleGroup
  az resource list --resource-group ExampleGroup
  ```

5. <span data-ttu-id="d3859-138">Při nasazování šablony můžete zadat parametry šablony jako parametry PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="d3859-138">You can specify template parameters as PowerShell parameters when deploying a template.</span></span> <span data-ttu-id="d3859-139">Předchozí příklad nezahrnoval žádné parametry šablony, takže se použily výchozí hodnoty z šablony.</span><span class="sxs-lookup"><span data-stu-id="d3859-139">The earlier example did not include any template parameters, so the default values in the template were used.</span></span> <span data-ttu-id="d3859-140">Pokud chcete nasadit další účet úložiště a zadat hodnoty parametrů pro předponu názvu úložiště a skladovou položku úložiště, použijte:</span><span class="sxs-lookup"><span data-stu-id="d3859-140">To deploy another storage account, and provide parameter values for the storage name prefix and the storage account SKU, use:</span></span>

  ```azurecli
  az group deployment create --name ExampleDeployment2 --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json --parameters '{"storageNamePrefix":{"value":"contoso"},"storageSKU":{"value":"Standard_GRS"}}'
  ```

  <span data-ttu-id="d3859-141">Nyní máte ve skupině prostředků dva účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="d3859-141">You now have two storage accounts in your resource group.</span></span> 

## <a name="example-storage-template"></a><span data-ttu-id="d3859-142">Příklad šablony úložiště</span><span class="sxs-lookup"><span data-stu-id="d3859-142">Example storage template</span></span>

<span data-ttu-id="d3859-143">Následující příklad šablony můžete použít k nasazení účtu úložiště do předplatného:</span><span class="sxs-lookup"><span data-stu-id="d3859-143">Use the following example template to deploy a storage account to your subscription:</span></span>

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
        "description": "The value to use for starting the storage account name."
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
        "description": "The type of replication to use for the storage account."
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

## <a name="next-steps"></a><span data-ttu-id="d3859-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d3859-144">Next steps</span></span>

* <span data-ttu-id="d3859-145">Podrobné informace o použití PowerShellu k nasazení šablon najdete v tématu [Nasazení prostředků pomocí šablon Resource Manageru a Azure PowerShellu](/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="d3859-145">For detailed information about using PowerShell to deploy templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](/azure/azure-resource-manager/resource-group-template-deploy).</span></span>
* <span data-ttu-id="d3859-146">Podrobné informace o použití Azure CLI k nasazení šablon najdete v tématu [Nasazení prostředků pomocí šablon Resource Manageru a Azure CLI](/azure/azure-resource-manager/resource-group-template-deploy-cli).</span><span class="sxs-lookup"><span data-stu-id="d3859-146">For detailed information about using Azure CLI to deploy templates, see [Deploy resources with Resource Manager templates and Azure CLI](/azure/azure-resource-manager/resource-group-template-deploy-cli).</span></span>



