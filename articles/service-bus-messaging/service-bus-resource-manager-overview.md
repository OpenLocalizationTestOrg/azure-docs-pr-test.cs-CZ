---
title: "prostředky Azure Service Bus aaaCreate pomocí šablony Azure Resource Manager | Microsoft Docs"
description: "Pomocí šablony Azure Resource Manager tooautomate hello vytváření prostředků služby Service Bus"
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
ms.openlocfilehash: e539902cae307b63ae7c332580e2064761331ec5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a><span data-ttu-id="f9089-103">Vytvoření služby Service Bus prostředků pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f9089-103">Create Service Bus resources using Azure Resource Manager templates</span></span>

<span data-ttu-id="f9089-104">Tento článek popisuje, jak toocreate a nasazení prostředků služby Service Bus pomocí šablony Azure Resource Manager, prostředí PowerShell a poskytovatele prostředků hello Service Bus.</span><span class="sxs-lookup"><span data-stu-id="f9089-104">This article describes how toocreate and deploy Service Bus resources using Azure Resource Manager templates, PowerShell, and hello Service Bus resource provider.</span></span>

<span data-ttu-id="f9089-105">Šablony Azure Resource Manageru můžete definovat hello toodeploy prostředky pro řešení a toospecify parametry a proměnné, které umožňují tooinput hodnoty pro různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="f9089-105">Azure Resource Manager templates help you define hello resources toodeploy for a solution, and toospecify parameters and variables that enable you tooinput values for different environments.</span></span> <span data-ttu-id="f9089-106">Šablona Hello se skládá z JSON a výrazy, můžete použít hodnoty tooconstruct pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="f9089-106">hello template consists of JSON and expressions that you can use tooconstruct values for your deployment.</span></span> <span data-ttu-id="f9089-107">Podrobné informace o vytváření šablon Azure Resource Manageru a diskuzi o formátu hello šablony najdete v tématu [syntaxi šablon Azure Resource Manager a struktura](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f9089-107">For detailed information about writing Azure Resource Manager templates, and a discussion of hello template format, see [structure and syntax of Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f9089-108">jak Hello příklady v této zobrazit článek toouse Azure Resource Manager toocreate oboru názvů Service Bus a zasílání zpráv entity (queue).</span><span class="sxs-lookup"><span data-stu-id="f9089-108">hello examples in this article show how toouse Azure Resource Manager toocreate a Service Bus namespace and messaging entity (queue).</span></span> <span data-ttu-id="f9089-109">Další příklady šablony najdete v článku hello [galerii šablon Azure rychlý Start] [ Azure Quickstart Templates gallery] a vyhledejte "Service Bus".</span><span class="sxs-lookup"><span data-stu-id="f9089-109">For other template examples, visit hello [Azure Quickstart Templates gallery][Azure Quickstart Templates gallery] and search for "Service Bus."</span></span>
>
>

## <a name="service-bus-resource-manager-templates"></a><span data-ttu-id="f9089-110">Šablony služby sběrnice Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f9089-110">Service Bus Resource Manager templates</span></span>

<span data-ttu-id="f9089-111">Tyto šablony správce prostředků Azure Service Bus jsou k dispozici ke stažení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="f9089-111">These Service Bus Azure Resource Manager templates are available for download and deployment.</span></span> <span data-ttu-id="f9089-112">Klikněte na tlačítko hello následující odkazy podrobnosti o každém z nich, se šablonami toohello odkazy na Githubu:</span><span class="sxs-lookup"><span data-stu-id="f9089-112">Click hello following links for details about each one, with links toohello templates on GitHub:</span></span>

* [<span data-ttu-id="f9089-113">Vytvoření oboru názvů Service Bus</span><span class="sxs-lookup"><span data-stu-id="f9089-113">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
* [<span data-ttu-id="f9089-114">Vytvoření oboru názvů Service Bus pomocí fronty</span><span class="sxs-lookup"><span data-stu-id="f9089-114">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
* [<span data-ttu-id="f9089-115">Vytvoření oboru názvů Service Bus s téma a odběr</span><span class="sxs-lookup"><span data-stu-id="f9089-115">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
* [<span data-ttu-id="f9089-116">Vytvoření oboru názvů Service Bus pomocí fronty a autorizační pravidla</span><span class="sxs-lookup"><span data-stu-id="f9089-116">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
* [<span data-ttu-id="f9089-117">Vytvoření oboru názvů Service Bus pomocí tématu, předplatné a pravidla</span><span class="sxs-lookup"><span data-stu-id="f9089-117">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)

## <a name="deploy-with-powershell"></a><span data-ttu-id="f9089-118">Nasazení s využitím PowerShellu</span><span class="sxs-lookup"><span data-stu-id="f9089-118">Deploy with PowerShell</span></span>

<span data-ttu-id="f9089-119">Hello následující postup popisuje, jak toouse prostředí PowerShell toodeploy šablonu Azure Resource Manager vytvářející **standardní** vrstvy oboru názvů Service Bus a fronty v daném oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="f9089-119">hello following procedure describes how toouse PowerShell toodeploy an Azure Resource Manager template that creates a **Standard** tier Service Bus namespace, and a queue within that namespace.</span></span> <span data-ttu-id="f9089-120">Tento příklad vychází z hello [vytvoření oboru názvů Service Bus s frontou](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) šablony.</span><span class="sxs-lookup"><span data-stu-id="f9089-120">This example is based on hello [Create a Service Bus namespace with queue](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) template.</span></span> <span data-ttu-id="f9089-121">Hello přibližnou pracovní postup je následující:</span><span class="sxs-lookup"><span data-stu-id="f9089-121">hello approximate workflow is as follows:</span></span>

1. <span data-ttu-id="f9089-122">Instalace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f9089-122">Install PowerShell.</span></span>
2. <span data-ttu-id="f9089-123">Vytvořte šablonu hello a (volitelně) ze souboru parametrů.</span><span class="sxs-lookup"><span data-stu-id="f9089-123">Create hello template and (optionally) a parameter file.</span></span>
3. <span data-ttu-id="f9089-124">V prostředí PowerShell Přihlaste se tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="f9089-124">In PowerShell, log in tooyour Azure account.</span></span>
4. <span data-ttu-id="f9089-125">Pokud žádný neexistuje, vytvořte novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="f9089-125">Create a new resource group if one does not exist.</span></span>
5. <span data-ttu-id="f9089-126">Test nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="f9089-126">Test hello deployment.</span></span>
6. <span data-ttu-id="f9089-127">V případě potřeby nastavte režim nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="f9089-127">If desired, set hello deployment mode.</span></span>
7. <span data-ttu-id="f9089-128">Nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="f9089-128">Deploy hello template.</span></span>

<span data-ttu-id="f9089-129">Úplné informace o nasazení šablony Azure Resource Manager najdete v tématu [nasazení prostředků pomocí šablony Azure Resource Manager][Deploy resources with Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="f9089-129">For complete information about deploying Azure Resource Manager templates, see [Deploy resources with Azure Resource Manager templates][Deploy resources with Azure Resource Manager templates].</span></span>

### <a name="install-powershell"></a><span data-ttu-id="f9089-130">Instalace PowerShellu</span><span class="sxs-lookup"><span data-stu-id="f9089-130">Install PowerShell</span></span>

<span data-ttu-id="f9089-131">Nainstalovat Azure PowerShell podle následujících pokynů hello v [Začínáme s Azure Powershellem](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="f9089-131">Install Azure PowerShell by following hello instructions in [Getting started with Azure PowerShell](/powershell/azure/get-started-azureps).</span></span>

### <a name="create-a-template"></a><span data-ttu-id="f9089-132">Vytvoření šablony</span><span class="sxs-lookup"><span data-stu-id="f9089-132">Create a template</span></span>

<span data-ttu-id="f9089-133">Klonování nebo kopírování hello [201-servicebus vytvořit queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) šablony z Githubu:</span><span class="sxs-lookup"><span data-stu-id="f9089-133">Clone or copy hello [201-servicebus-create-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) template from GitHub:</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of hello Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of hello Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by hello template"
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

### <a name="create-a-parameters-file-optional"></a><span data-ttu-id="f9089-134">Vytvořte soubor parametrů (volitelné)</span><span class="sxs-lookup"><span data-stu-id="f9089-134">Create a parameters file (optional)</span></span>

<span data-ttu-id="f9089-135">toouse soubor volitelné parametry kopie hello [201-servicebus vytvořit queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) souboru.</span><span class="sxs-lookup"><span data-stu-id="f9089-135">toouse an optional parameters file, copy hello [201-servicebus-create-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) file.</span></span> <span data-ttu-id="f9089-136">Nahraďte hodnotu hello `serviceBusNamespaceName` hello název oboru názvů Service Bus hello chcete toocreate v tomto nasazení a nahraďte hodnotu hello `serviceBusQueueName` s názvem hello hello fronty chcete toocreate.</span><span class="sxs-lookup"><span data-stu-id="f9089-136">Replace hello value of `serviceBusNamespaceName` with hello name of hello Service Bus namespace you want toocreate in this deployment, and replace hello value of `serviceBusQueueName` with hello name of hello queue you want toocreate.</span></span>

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

<span data-ttu-id="f9089-137">Další informace najdete v tématu hello [parametry](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) tématu.</span><span class="sxs-lookup"><span data-stu-id="f9089-137">For more information, see hello [Parameters](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) topic.</span></span>

### <a name="log-in-tooazure-and-set-hello-azure-subscription"></a><span data-ttu-id="f9089-138">Přihlaste se tooAzure a nastavte hello předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="f9089-138">Log in tooAzure and set hello Azure subscription</span></span>

<span data-ttu-id="f9089-139">Příkazovém řádku prostředí PowerShell spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="f9089-139">From a PowerShell prompt, run hello following command:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="f9089-140">Jste výzvami toolog na tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="f9089-140">You are prompted toolog on tooyour Azure account.</span></span> <span data-ttu-id="f9089-141">Po přihlášení, spusťte následující příkaz tooview hello dostupných předplatných.</span><span class="sxs-lookup"><span data-stu-id="f9089-141">After logging on, run hello following command tooview your available subscriptions.</span></span>

```powershell
Get-AzureRMSubscription
```

<span data-ttu-id="f9089-142">Tento příkaz vrátí seznam dostupných předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="f9089-142">This command returns a list of available Azure subscriptions.</span></span> <span data-ttu-id="f9089-143">Zvolte předplatné pro hello aktuální relace tak, že spustíte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="f9089-143">Choose a subscription for hello current session by running hello following command.</span></span> <span data-ttu-id="f9089-144">Nahraďte `<YourSubscriptionId>` s hello identifikátor GUID pro hello předplatné chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="f9089-144">Replace `<YourSubscriptionId>` with hello GUID for hello Azure subscription you want toouse.</span></span>

```powershell
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-hello-resource-group"></a><span data-ttu-id="f9089-145">Skupina prostředků hello sady</span><span class="sxs-lookup"><span data-stu-id="f9089-145">Set hello resource group</span></span>

<span data-ttu-id="f9089-146">Pokud nemáte existující prostředek skupiny, vytvořte novou skupinu prostředků s hello ** New-AzureRmResourceGroup ** příkaz.</span><span class="sxs-lookup"><span data-stu-id="f9089-146">If you do not have an existing resource group, create a new resource group with hello **New-AzureRmResourceGroup ** command.</span></span> <span data-ttu-id="f9089-147">Zadejte název hello hello skupinu prostředků a umístění, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="f9089-147">Provide hello name of hello resource group and location you want toouse.</span></span> <span data-ttu-id="f9089-148">Například:</span><span class="sxs-lookup"><span data-stu-id="f9089-148">For example:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

<span data-ttu-id="f9089-149">Pokud bylo úspěšné, zobrazí se souhrn hello novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="f9089-149">If successful, a summary of hello new resource group is displayed.</span></span>

```powershell
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-hello-deployment"></a><span data-ttu-id="f9089-150">Testovací nasazení pro hello</span><span class="sxs-lookup"><span data-stu-id="f9089-150">Test hello deployment</span></span>

<span data-ttu-id="f9089-151">Ověření nasazení spuštěním hello `Test-AzureRmResourceGroupDeployment` rutiny.</span><span class="sxs-lookup"><span data-stu-id="f9089-151">Validate your deployment by running hello `Test-AzureRmResourceGroupDeployment` cmdlet.</span></span> <span data-ttu-id="f9089-152">Při testování hello nasazení, zadejte přesně tak, jak by při provádění nasazení hello parametry.</span><span class="sxs-lookup"><span data-stu-id="f9089-152">When testing hello deployment, provide parameters exactly as you would when executing hello deployment.</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

### <a name="create-hello-deployment"></a><span data-ttu-id="f9089-153">Vytvoření nasazení hello</span><span class="sxs-lookup"><span data-stu-id="f9089-153">Create hello deployment</span></span>

<span data-ttu-id="f9089-154">nové nasazení hello toocreate, spusťte hello `New-AzureRmResourceGroupDeployment` rutiny a zadejte potřebné parametry hello po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="f9089-154">toocreate hello new deployment, run hello `New-AzureRmResourceGroupDeployment` cmdlet, and provide hello necessary parameters when prompted.</span></span> <span data-ttu-id="f9089-155">Hello parametry jsou název pro vaše nasazení hello název vaší skupiny prostředků a hello cesta nebo adresa URL souboru šablony toohello.</span><span class="sxs-lookup"><span data-stu-id="f9089-155">hello parameters include a name for your deployment, hello name of your resource group, and hello path or URL toohello template file.</span></span> <span data-ttu-id="f9089-156">Pokud hello **režimu** není zadán parametr, hello výchozí hodnotu **přírůstkové** se používá.</span><span class="sxs-lookup"><span data-stu-id="f9089-156">If hello **Mode** parameter is not specified, hello default value of **Incremental** is used.</span></span> <span data-ttu-id="f9089-157">Další informace najdete v tématu [přírůstkové a úplné nasazení](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments).</span><span class="sxs-lookup"><span data-stu-id="f9089-157">For more information, see [Incremental and complete deployments](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments).</span></span>

<span data-ttu-id="f9089-158">Hello následující příkazové řádky můžete pro hello tři parametry v okně PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="f9089-158">hello following command prompts you for hello three parameters in hello PowerShell window:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

<span data-ttu-id="f9089-159">toospecify soubor parametrů místo toho použijte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="f9089-159">toospecify a parameters file instead, use hello following command.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json -TemplateParameterFile <path tooparameters file>\azuredeploy.parameters.json
```

<span data-ttu-id="f9089-160">Vložené parametry můžete použít také při spuštění rutiny nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="f9089-160">You can also use inline parameters when you run hello deployment cmdlet.</span></span> <span data-ttu-id="f9089-161">příkaz Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="f9089-161">hello command is as follows:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json -parameterName "parameterValue"
```

<span data-ttu-id="f9089-162">toorun [dokončení](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) nasazení, sada hello **režimu** parametr příliš**Complete**:</span><span class="sxs-lookup"><span data-stu-id="f9089-162">toorun a [complete](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) deployment, set hello **Mode** parameter too**Complete**:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

### <a name="verify-hello-deployment"></a><span data-ttu-id="f9089-163">Ověření nasazení hello</span><span class="sxs-lookup"><span data-stu-id="f9089-163">Verify hello deployment</span></span>
<span data-ttu-id="f9089-164">Pokud hello prostředky nasadí úspěšně, zobrazí se v okně PowerShell hello shrnutí nasazení hello:</span><span class="sxs-lookup"><span data-stu-id="f9089-164">If hello resources are deployed successfully, a summary of hello deployment is displayed in hello PowerShell window:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="f9089-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f9089-165">Next steps</span></span>
<span data-ttu-id="f9089-166">Nyní jste se seznámili hello základní pracovní postup a příkazy pro nasazení šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f9089-166">You've now seen hello basic workflow and commands for deploying an Azure Resource Manager template.</span></span> <span data-ttu-id="f9089-167">Podrobnější informace najdete v článku hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="f9089-167">For more detailed information, visit hello following links:</span></span>

* <span data-ttu-id="f9089-168">[Přehled Azure Resource Manageru][Azure Resource Manager overview]</span><span class="sxs-lookup"><span data-stu-id="f9089-168">[Azure Resource Manager overview][Azure Resource Manager overview]</span></span>
* <span data-ttu-id="f9089-169">[Nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell][Deploy resources with Azure Resource Manager templates]</span><span class="sxs-lookup"><span data-stu-id="f9089-169">[Deploy resources with Resource Manager templates and Azure PowerShell][Deploy resources with Azure Resource Manager templates]</span></span>
* [<span data-ttu-id="f9089-170">Tvorba šablon Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="f9089-170">Authoring Azure Resource Manager templates</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)

[Azure Resource Manager overview]: ../azure-resource-manager/resource-group-overview.md
[Deploy resources with Azure Resource Manager templates]: ../azure-resource-manager/resource-group-template-deploy.md
[Azure Quickstart Templates gallery]: https://azure.microsoft.com/documentation/templates/?term=service+bus
