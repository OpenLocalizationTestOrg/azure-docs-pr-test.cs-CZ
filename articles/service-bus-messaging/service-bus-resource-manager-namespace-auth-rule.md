---
title: "Vytvoření pravidla autorizace služby Service Bus pomocí šablony Azure Resource Manager | Microsoft Docs"
description: "Vytvoření pravidla autorizace sběrnice pro obor názvů a fronty pomocí šablony Azure Resource Manageru"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 7f1443a0-5fa8-4d90-8637-1a977ef0b1f0
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: fbd2372829a1aefa2c080c0a8a72b9ff4375b16f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a><span data-ttu-id="926ad-103">Vytvoření pravidla autorizace sběrnice pro obor názvů a fronty pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="926ad-103">Create a Service Bus authorization rule for namespace and queue using an Azure Resource Manager template</span></span>

<span data-ttu-id="926ad-104">Tento článek ukazuje, jak používat šablonu Azure Resource Manager, který vytváří [autorizační pravidlo](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) oboru názvů Service Bus a fronty.</span><span class="sxs-lookup"><span data-stu-id="926ad-104">This article shows how to use an Azure Resource Manager template that creates an [authorization rule](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) for a Service Bus namespace and queue.</span></span> <span data-ttu-id="926ad-105">Se dozvíte, jak definovat prostředky, ke kterým nasazených a jak definovat parametry, které jsou zadané, když se spustí nasazení.</span><span class="sxs-lookup"><span data-stu-id="926ad-105">You will learn how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="926ad-106">Tuto šablonu můžete použít pro vlastní nasazení nebo ji upravit, aby splňovala vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="926ad-106">You can use this template for your own deployments, or customize it to meet your requirements.</span></span>

<span data-ttu-id="926ad-107">Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="926ad-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="926ad-108">Úplnou šablonu, najdete v článku [šablonu pravidla autorizace služby Service Bus] [ Service Bus auth rule template] na Githubu.</span><span class="sxs-lookup"><span data-stu-id="926ad-108">For the complete template, see the [Service Bus authorization rule template][Service Bus auth rule template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="926ad-109">Následující šablony Azure Resource Manager jsou k dispozici ke stažení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="926ad-109">The following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="926ad-110">Vytvoření oboru názvů Service Bus</span><span class="sxs-lookup"><span data-stu-id="926ad-110">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="926ad-111">Vytvoření oboru názvů Service Bus pomocí fronty</span><span class="sxs-lookup"><span data-stu-id="926ad-111">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="926ad-112">Vytvoření oboru názvů Service Bus s téma a odběr</span><span class="sxs-lookup"><span data-stu-id="926ad-112">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
> * [<span data-ttu-id="926ad-113">Vytvoření oboru názvů Service Bus pomocí tématu, předplatné a pravidla</span><span class="sxs-lookup"><span data-stu-id="926ad-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="926ad-114">Vyhledat nejnovější šablony, najdete [šablon Azure rychlý Start] [ Azure Quickstart Templates] galerie a vyhledejte "Service Bus".</span><span class="sxs-lookup"><span data-stu-id="926ad-114">To check for the latest templates, visit the [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for "Service Bus."</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="926ad-115">Co budete nasazovat?</span><span class="sxs-lookup"><span data-stu-id="926ad-115">What will you deploy?</span></span>
<span data-ttu-id="926ad-116">Pomocí této šablony nasadíte Service Bus autorizační pravidlo pro obor názvů a entity přenosu zpráv (v tomto případě fronty).</span><span class="sxs-lookup"><span data-stu-id="926ad-116">With this template, you will deploy a Service Bus authorization rule for a namespace and messaging entity (in this case, a queue).</span></span>

<span data-ttu-id="926ad-117">Tato šablona používá [sdíleného přístupového podpisu (SAS)](service-bus-sas.md) pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="926ad-117">This template uses [Shared Access Signature (SAS)](service-bus-sas.md) for authentication.</span></span> <span data-ttu-id="926ad-118">SAS umožňuje aplikacím ke svému ověření u služby Service Bus pomocí přístupový klíč konfigurovány v oboru názvů, nebo u entity zasílání zpráv (fronty nebo téma) ke které jsou přidružené konkrétní práva.</span><span class="sxs-lookup"><span data-stu-id="926ad-118">SAS enables applications to authenticate to Service Bus using an access key configured on the namespace, or on the messaging entity (queue or topic) with which specific rights are associated.</span></span> <span data-ttu-id="926ad-119">Pak můžete tento klíč k vygenerování tokenu SAS, který zase můžou klienti používat ke svému ověření u služby Service Bus.</span><span class="sxs-lookup"><span data-stu-id="926ad-119">You can then use this key to generate a SAS token that clients can in turn use to authenticate to Service Bus.</span></span>

<span data-ttu-id="926ad-120">Pokud chcete nasazení spustit automaticky, klikněte na následující tlačítko:</span><span class="sxs-lookup"><span data-stu-id="926ad-120">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="926ad-121">[![Nasazení do Azure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="926ad-121">[![Deploy to Azure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="926ad-122">Parametry</span><span class="sxs-lookup"><span data-stu-id="926ad-122">Parameters</span></span>

<span data-ttu-id="926ad-123">Pomocí Azure Resource Manageru definujete parametry pro hodnoty, které chcete zadat při nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="926ad-123">With Azure Resource Manager, you define parameters for values you want to specify when the template is deployed.</span></span> <span data-ttu-id="926ad-124">Šablona obsahuje oddíl s názvem `Parameters` obsahující všechny hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="926ad-124">The template includes a section called `Parameters` that contains all of the parameter values.</span></span> <span data-ttu-id="926ad-125">Měli byste parametr pro ty hodnoty, které se liší podle prostředí, ve kterém provádíte nasazení nebo na základě projektu, které nasazujete.</span><span class="sxs-lookup"><span data-stu-id="926ad-125">You should define a parameter for those values that will vary based on the project you are deploying or based on the environment you are deploying to.</span></span> <span data-ttu-id="926ad-126">Nedefinují parametry pro hodnoty, které zůstanou vždy stejná.</span><span class="sxs-lookup"><span data-stu-id="926ad-126">Do not define parameters for values that will always stay the same.</span></span> <span data-ttu-id="926ad-127">Každá hodnota parametru se v šabloně použije k definování nasazovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="926ad-127">Each parameter value is used in the template to define the resources that are deployed.</span></span>

<span data-ttu-id="926ad-128">Šablona definuje následující parametry.</span><span class="sxs-lookup"><span data-stu-id="926ad-128">The template defines the following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="926ad-129">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="926ad-129">serviceBusNamespaceName</span></span>
<span data-ttu-id="926ad-130">Název oboru názvů Service Bus k vytvoření.</span><span class="sxs-lookup"><span data-stu-id="926ad-130">The name of the Service Bus namespace to create.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="namespaceauthorizationrulename"></a><span data-ttu-id="926ad-131">namespaceAuthorizationRuleName</span><span class="sxs-lookup"><span data-stu-id="926ad-131">namespaceAuthorizationRuleName</span></span>
<span data-ttu-id="926ad-132">Název autorizační pravidlo pro obor názvů.</span><span class="sxs-lookup"><span data-stu-id="926ad-132">The name of the authorization rule for the namespace.</span></span>

```json
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a><span data-ttu-id="926ad-133">serviceBusQueueName</span><span class="sxs-lookup"><span data-stu-id="926ad-133">serviceBusQueueName</span></span>
<span data-ttu-id="926ad-134">Název fronty v oboru názvů Service Bus.</span><span class="sxs-lookup"><span data-stu-id="926ad-134">The name of the queue in the Service Bus namespace.</span></span>

```json
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a><span data-ttu-id="926ad-135">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="926ad-135">serviceBusApiVersion</span></span>
<span data-ttu-id="926ad-136">Verze rozhraní API služby Service Bus šablony.</span><span class="sxs-lookup"><span data-stu-id="926ad-136">The Service Bus API version of the template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a><span data-ttu-id="926ad-137">Prostředky k nasazení</span><span class="sxs-lookup"><span data-stu-id="926ad-137">Resources to deploy</span></span>
<span data-ttu-id="926ad-138">Vytvoří standardní oboru názvů Service Bus typu **zasílání zpráv**a Service Bus autorizační pravidlo pro obor názvů a entity.</span><span class="sxs-lookup"><span data-stu-id="926ad-138">Creates a standard Service Bus namespace of type **Messaging**, and a Service Bus authorization rule for namespace and entity.</span></span>

```json
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

## <a name="commands-to-run-deployment"></a><span data-ttu-id="926ad-139">Příkazy pro spuštění nasazení</span><span class="sxs-lookup"><span data-stu-id="926ad-139">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="926ad-140">PowerShell</span><span class="sxs-lookup"><span data-stu-id="926ad-140">PowerShell</span></span>
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="926ad-141">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="926ad-141">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="926ad-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="926ad-142">Next steps</span></span>
<span data-ttu-id="926ad-143">Teď, když jste vytvoření a nasazení prostředků pomocí Azure Resource Manager, zjistěte, jak tyto zdroje spravovat pomocí těchto článků:</span><span class="sxs-lookup"><span data-stu-id="926ad-143">Now that you've created and deployed resources using Azure Resource Manager, learn how to manage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="926ad-144">Správa služby Service Bus pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="926ad-144">Manage Service Bus with PowerShell</span></span>](service-bus-powershell-how-to-provision.md)
* [<span data-ttu-id="926ad-145">Správa prostředků služby Service Bus pomocí Průzkumníka služby sběrnice</span><span class="sxs-lookup"><span data-stu-id="926ad-145">Manage Service Bus resources with the Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)
* [<span data-ttu-id="926ad-146">Service Bus ověřování a autorizace</span><span class="sxs-lookup"><span data-stu-id="926ad-146">Service Bus authentication and authorization</span></span>](service-bus-authentication-and-authorization.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus auth rule template]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
