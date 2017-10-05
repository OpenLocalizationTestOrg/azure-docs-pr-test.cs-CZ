---
title: "Vytvořit prostředky Azure Service Bus pomocí šablony Azure Resource Manager | Microsoft Docs"
description: "Použití šablon Azure Resource Manageru k automatizaci vytváření prostředků služby Service Bus"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 24f6a207-0fa4-49cf-8a58-963f9e2fd655
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: c8142d8edfd3a527b13d655bac21acf5332f2d14
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a><span data-ttu-id="9877d-103">Vytvoření služby Service Bus prostředků pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9877d-103">Create Service Bus resources using Azure Resource Manager templates</span></span>

<span data-ttu-id="9877d-104">Tento článek popisuje postup vytvoření a nasazení prostředků služby Service Bus pomocí šablony Azure Resource Manager, prostředí PowerShell a zprostředkovatele prostředků služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="9877d-104">This article describes how to create and deploy Service Bus resources using Azure Resource Manager templates, PowerShell, and the Service Bus resource provider.</span></span>

<span data-ttu-id="9877d-105">Šablony Azure Resource Manageru můžete definovat, které prostředky pro řešení nasadit a určit parametry a proměnné, které vám umožní zadat hodnoty pro různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="9877d-105">Azure Resource Manager templates help you define the resources to deploy for a solution, and to specify parameters and variables that enable you to input values for different environments.</span></span> <span data-ttu-id="9877d-106">Šablona se skládá z JSON a výrazy, které můžete použít k vytvoření hodnot pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="9877d-106">The template consists of JSON and expressions that you can use to construct values for your deployment.</span></span> <span data-ttu-id="9877d-107">Podrobné informace o vytváření šablon Azure Resource Manageru a diskuzi o formátu šablony najdete v tématu [syntaxi šablon Azure Resource Manager a struktura](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="9877d-107">For detailed information about writing Azure Resource Manager templates, and a discussion of the template format, see [structure and syntax of Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

> [!NOTE]
> <span data-ttu-id="9877d-108">V příkladech v tomto článku ukazují, jak pomocí Správce prostředků Azure k vytvoření oboru názvů Service Bus a entity zasílání zpráv (fronty).</span><span class="sxs-lookup"><span data-stu-id="9877d-108">The examples in this article show how to use Azure Resource Manager to create a Service Bus namespace and messaging entity (queue).</span></span> <span data-ttu-id="9877d-109">Další příklady šablony najdete v článku [galerii šablon Azure rychlý Start] [ Azure Quickstart Templates gallery] a vyhledejte "Service Bus".</span><span class="sxs-lookup"><span data-stu-id="9877d-109">For other template examples, visit the [Azure Quickstart Templates gallery][Azure Quickstart Templates gallery] and search for "Service Bus."</span></span>
>
>

## <a name="service-bus-resource-manager-templates"></a><span data-ttu-id="9877d-110">Šablony služby sběrnice Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9877d-110">Service Bus Resource Manager templates</span></span>

<span data-ttu-id="9877d-111">Tyto šablony správce prostředků Azure Service Bus jsou k dispozici ke stažení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="9877d-111">These Service Bus Azure Resource Manager templates are available for download and deployment.</span></span> <span data-ttu-id="9877d-112">Kliknutím na následující odkazy podrobnosti o každém z nich, s odkazy na šablony na Githubu:</span><span class="sxs-lookup"><span data-stu-id="9877d-112">Click the following links for details about each one, with links to the templates on GitHub:</span></span>

* [<span data-ttu-id="9877d-113">Vytvoření oboru názvů Service Bus</span><span class="sxs-lookup"><span data-stu-id="9877d-113">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
* [<span data-ttu-id="9877d-114">Vytvoření oboru názvů Service Bus pomocí fronty</span><span class="sxs-lookup"><span data-stu-id="9877d-114">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
* [<span data-ttu-id="9877d-115">Vytvoření oboru názvů Service Bus s téma a odběr</span><span class="sxs-lookup"><span data-stu-id="9877d-115">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
* [<span data-ttu-id="9877d-116">Vytvoření oboru názvů Service Bus pomocí fronty a autorizační pravidla</span><span class="sxs-lookup"><span data-stu-id="9877d-116">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
* [<span data-ttu-id="9877d-117">Vytvoření oboru názvů Service Bus pomocí tématu, předplatné a pravidla</span><span class="sxs-lookup"><span data-stu-id="9877d-117">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)

## <a name="deploy-with-powershell"></a><span data-ttu-id="9877d-118">Nasazení s využitím PowerShellu</span><span class="sxs-lookup"><span data-stu-id="9877d-118">Deploy with PowerShell</span></span>

<span data-ttu-id="9877d-119">Následující postup popisuje, jak pomocí prostředí PowerShell pro nasazení šablonu Azure Resource Manager, která vytvoří **standardní** vrstvy oboru názvů Service Bus a fronty v daném oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="9877d-119">The following procedure describes how to use PowerShell to deploy an Azure Resource Manager template that creates a **Standard** tier Service Bus namespace, and a queue within that namespace.</span></span> <span data-ttu-id="9877d-120">Tento příklad vychází z [vytvoření oboru názvů Service Bus s frontou](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) šablony.</span><span class="sxs-lookup"><span data-stu-id="9877d-120">This example is based on the [Create a Service Bus namespace with queue](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) template.</span></span> <span data-ttu-id="9877d-121">Přibližná pracovní postup je následující:</span><span class="sxs-lookup"><span data-stu-id="9877d-121">The approximate workflow is as follows:</span></span>

1. <span data-ttu-id="9877d-122">Instalace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9877d-122">Install PowerShell.</span></span>
2. <span data-ttu-id="9877d-123">Vytvořte šablonu a (volitelně) ze souboru parametrů.</span><span class="sxs-lookup"><span data-stu-id="9877d-123">Create the template and (optionally) a parameter file.</span></span>
3. <span data-ttu-id="9877d-124">V prostředí PowerShell Přihlaste se k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="9877d-124">In PowerShell, log in to your Azure account.</span></span>
4. <span data-ttu-id="9877d-125">Pokud žádný neexistuje, vytvořte novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="9877d-125">Create a new resource group if one does not exist.</span></span>
5. <span data-ttu-id="9877d-126">Testovací nasazení.</span><span class="sxs-lookup"><span data-stu-id="9877d-126">Test the deployment.</span></span>
6. <span data-ttu-id="9877d-127">V případě potřeby nastavte režim nasazení.</span><span class="sxs-lookup"><span data-stu-id="9877d-127">If desired, set the deployment mode.</span></span>
7. <span data-ttu-id="9877d-128">Nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="9877d-128">Deploy the template.</span></span>

<span data-ttu-id="9877d-129">Úplné informace o nasazení šablony Azure Resource Manager najdete v tématu [nasazení prostředků pomocí šablony Azure Resource Manager][Deploy resources with Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="9877d-129">For complete information about deploying Azure Resource Manager templates, see [Deploy resources with Azure Resource Manager templates][Deploy resources with Azure Resource Manager templates].</span></span>

### <a name="install-powershell"></a><span data-ttu-id="9877d-130">Instalace PowerShellu</span><span class="sxs-lookup"><span data-stu-id="9877d-130">Install PowerShell</span></span>

<span data-ttu-id="9877d-131">Nainstalovat Azure PowerShell podle pokynů v [Začínáme s Azure Powershellem](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="9877d-131">Install Azure PowerShell by following the instructions in [Getting started with Azure PowerShell](/powershell/azure/get-started-azureps).</span></span>

### <a name="create-a-template"></a><span data-ttu-id="9877d-132">Vytvoření šablony</span><span class="sxs-lookup"><span data-stu-id="9877d-132">Create a template</span></span>

<span data-ttu-id="9877d-133">Klonování nebo kopírování [201-servicebus vytvořit queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) šablony z Githubu:</span><span class="sxs-lookup"><span data-stu-id="9877d-133">Clone or copy the [201-servicebus-create-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) template from GitHub:</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by the template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a><span data-ttu-id="9877d-134">Vytvořte soubor parametrů (volitelné)</span><span class="sxs-lookup"><span data-stu-id="9877d-134">Create a parameters file (optional)</span></span>

<span data-ttu-id="9877d-135">Chcete-li použít soubor volitelné parametry, zkopírujte [201-servicebus vytvořit queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) souboru.</span><span class="sxs-lookup"><span data-stu-id="9877d-135">To use an optional parameters file, copy the [201-servicebus-create-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) file.</span></span> <span data-ttu-id="9877d-136">Nahraďte hodnotu `serviceBusNamespaceName` s názvem oboru názvů Service Bus, kterou chcete vytvořit v tomto nasazení a nahraďte hodnotu `serviceBusQueueName` s názvem fronty, kterou chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="9877d-136">Replace the value of `serviceBusNamespaceName` with the name of the Service Bus namespace you want to create in this deployment, and replace the value of `serviceBusQueueName` with the name of the queue you want to create.</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

<span data-ttu-id="9877d-137">Další informace najdete v tématu [parametry](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) tématu.</span><span class="sxs-lookup"><span data-stu-id="9877d-137">For more information, see the [Parameters](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) topic.</span></span>

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a><span data-ttu-id="9877d-138">Přihlaste se k Azure a nastavte předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="9877d-138">Log in to Azure and set the Azure subscription</span></span>

<span data-ttu-id="9877d-139">Z řádku prostředí PowerShell spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9877d-139">From a PowerShell prompt, run the following command:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="9877d-140">Zobrazí se výzva k přihlášení k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="9877d-140">You are prompted to log on to your Azure account.</span></span> <span data-ttu-id="9877d-141">Po přihlášení, spusťte následující příkaz k zobrazení dostupných předplatných.</span><span class="sxs-lookup"><span data-stu-id="9877d-141">After logging on, run the following command to view your available subscriptions.</span></span>

```powershell
Get-AzureRMSubscription
```

<span data-ttu-id="9877d-142">Tento příkaz vrátí seznam dostupných předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="9877d-142">This command returns a list of available Azure subscriptions.</span></span> <span data-ttu-id="9877d-143">Spuštěním následujícího příkazu vyberte předplatné pro aktuální relaci.</span><span class="sxs-lookup"><span data-stu-id="9877d-143">Choose a subscription for the current session by running the following command.</span></span> <span data-ttu-id="9877d-144">Nahraďte `<YourSubscriptionId>` s identifikátorem GUID pro předplatné Azure, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="9877d-144">Replace `<YourSubscriptionId>` with the GUID for the Azure subscription you want to use.</span></span>

```powershell
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a><span data-ttu-id="9877d-145">Nastavit skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="9877d-145">Set the resource group</span></span>

<span data-ttu-id="9877d-146">Pokud nemáte existující prostředek skupiny, vytvořte novou skupinu prostředků s ** New-AzureRmResourceGroup ** příkaz.</span><span class="sxs-lookup"><span data-stu-id="9877d-146">If you do not have an existing resource group, create a new resource group with the **New-AzureRmResourceGroup ** command.</span></span> <span data-ttu-id="9877d-147">Zadejte název skupiny prostředků a umístění, do kterého chcete použít.</span><span class="sxs-lookup"><span data-stu-id="9877d-147">Provide the name of the resource group and location you want to use.</span></span> <span data-ttu-id="9877d-148">Například:</span><span class="sxs-lookup"><span data-stu-id="9877d-148">For example:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

<span data-ttu-id="9877d-149">Pokud bylo úspěšné, zobrazí se souhrn novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="9877d-149">If successful, a summary of the new resource group is displayed.</span></span>

```powershell
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a><span data-ttu-id="9877d-150">Otestování nasazení</span><span class="sxs-lookup"><span data-stu-id="9877d-150">Test the deployment</span></span>

<span data-ttu-id="9877d-151">Ověření nasazení tak, že spustíte `Test-AzureRmResourceGroupDeployment` rutiny.</span><span class="sxs-lookup"><span data-stu-id="9877d-151">Validate your deployment by running the `Test-AzureRmResourceGroupDeployment` cmdlet.</span></span> <span data-ttu-id="9877d-152">Při testování nasazení, zadejte parametry přesně stejně jako při provádění nasazení.</span><span class="sxs-lookup"><span data-stu-id="9877d-152">When testing the deployment, provide parameters exactly as you would when executing the deployment.</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a><span data-ttu-id="9877d-153">Vytvoření nasazení</span><span class="sxs-lookup"><span data-stu-id="9877d-153">Create the deployment</span></span>

<span data-ttu-id="9877d-154">Chcete-li vytvořit nové nasazení, spusťte `New-AzureRmResourceGroupDeployment` rutiny a zadejte potřebné parametry po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="9877d-154">To create the new deployment, run the `New-AzureRmResourceGroupDeployment` cmdlet, and provide the necessary parameters when prompted.</span></span> <span data-ttu-id="9877d-155">Parametry jsou název pro vaše nasazení, název vaší skupiny prostředků a cesta nebo adresa URL k souboru šablony.</span><span class="sxs-lookup"><span data-stu-id="9877d-155">The parameters include a name for your deployment, the name of your resource group, and the path or URL to the template file.</span></span> <span data-ttu-id="9877d-156">Pokud **režimu** není zadán parametr, výchozí hodnota **přírůstkové** se používá.</span><span class="sxs-lookup"><span data-stu-id="9877d-156">If the **Mode** parameter is not specified, the default value of **Incremental** is used.</span></span> <span data-ttu-id="9877d-157">Další informace najdete v tématu [přírůstkové a úplné nasazení](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments).</span><span class="sxs-lookup"><span data-stu-id="9877d-157">For more information, see [Incremental and complete deployments](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments).</span></span>

<span data-ttu-id="9877d-158">Následující příkaz vás vyzve k zadání tři parametry v okně prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="9877d-158">The following command prompts you for the three parameters in the PowerShell window:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

<span data-ttu-id="9877d-159">Místo toho zadat soubor parametrů, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="9877d-159">To specify a parameters file instead, use the following command.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

<span data-ttu-id="9877d-160">Vložené parametry můžete použít také při spuštění rutiny nasazení.</span><span class="sxs-lookup"><span data-stu-id="9877d-160">You can also use inline parameters when you run the deployment cmdlet.</span></span> <span data-ttu-id="9877d-161">Příkaz vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="9877d-161">The command is as follows:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

<span data-ttu-id="9877d-162">Ke spuštění [dokončení](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) nasazení, nastavte **režimu** parametru **Complete**:</span><span class="sxs-lookup"><span data-stu-id="9877d-162">To run a [complete](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) deployment, set the **Mode** parameter to **Complete**:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="verify-the-deployment"></a><span data-ttu-id="9877d-163">Ověření nasazení</span><span class="sxs-lookup"><span data-stu-id="9877d-163">Verify the deployment</span></span>
<span data-ttu-id="9877d-164">Pokud prostředky jsou nasazeny úspěšně, zobrazí se souhrn nasazení v okně prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="9877d-164">If the resources are deployed successfully, a summary of the deployment is displayed in the PowerShell window:</span></span>

```powershell
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a><span data-ttu-id="9877d-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9877d-165">Next steps</span></span>
<span data-ttu-id="9877d-166">Nyní jste se seznámili základní pracovní postup a příkazy pro nasazení šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9877d-166">You've now seen the basic workflow and commands for deploying an Azure Resource Manager template.</span></span> <span data-ttu-id="9877d-167">Podrobnější informace získáte pomocí následujících odkazů:</span><span class="sxs-lookup"><span data-stu-id="9877d-167">For more detailed information, visit the following links:</span></span>

* <span data-ttu-id="9877d-168">[Přehled Azure Resource Manageru][Azure Resource Manager overview]</span><span class="sxs-lookup"><span data-stu-id="9877d-168">[Azure Resource Manager overview][Azure Resource Manager overview]</span></span>
* <span data-ttu-id="9877d-169">[Nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell][Deploy resources with Azure Resource Manager templates]</span><span class="sxs-lookup"><span data-stu-id="9877d-169">[Deploy resources with Resource Manager templates and Azure PowerShell][Deploy resources with Azure Resource Manager templates]</span></span>
* [<span data-ttu-id="9877d-170">Tvorba šablon Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="9877d-170">Authoring Azure Resource Manager templates</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)

[Azure Resource Manager overview]: ../azure-resource-manager/resource-group-overview.md
[Deploy resources with Azure Resource Manager templates]: ../azure-resource-manager/resource-group-template-deploy.md
[Azure Quickstart Templates gallery]: https://azure.microsoft.com/documentation/templates/?term=service+bus
