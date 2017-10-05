---
title: "Vytvořit odběr tématu obor názvů sběrnice služby Azure pomocí šablony Azure Resource Manager | Microsoft Docs"
description: "Vytvoření oboru názvů Service Bus s téma a odběr pomocí šablony Azure Resource Manageru"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: d3d55200-5c60-4b5f-822d-59974cafff0e
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: 8dd48787e7b788d249085b3110484de1a2c1d265
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-service-bus-namespace-with-topic-and-subscription-using-an-azure-resource-manager-template"></a><span data-ttu-id="354f9-103">Vytvoření oboru názvů Service Bus s téma a odběr pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="354f9-103">Create a Service Bus namespace with topic and subscription using an Azure Resource Manager template</span></span>

<span data-ttu-id="354f9-104">Tento článek ukazuje, jak používat šablonu Azure Resource Manager, který vytvoří obor názvů sběrnice a téma a odběr v daném oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="354f9-104">This article shows how to use an Azure Resource Manager template that creates a Service Bus namespace and a topic and subscription within that namespace.</span></span> <span data-ttu-id="354f9-105">Se dozvíte, jak definovat prostředky, ke kterým nasazených a jak definovat parametry, které jsou zadané, když se spustí nasazení.</span><span class="sxs-lookup"><span data-stu-id="354f9-105">You will learn how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="354f9-106">Tuto šablonu můžete použít pro vlastní nasazení nebo ji upravit, aby splňovala vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="354f9-106">You can use this template for your own deployments, or customize it to meet your requirements</span></span>

<span data-ttu-id="354f9-107">Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="354f9-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="354f9-108">Úplnou šablonu, najdete v článku [oboru názvů Service Bus s téma a odběr] [ Service Bus namespace with topic and subscription] šablony.</span><span class="sxs-lookup"><span data-stu-id="354f9-108">For the complete template, see the [Service Bus namespace with topic and subscription][Service Bus namespace with topic and subscription] template.</span></span>

> [!NOTE]
> <span data-ttu-id="354f9-109">Následující šablony Azure Resource Manager jsou k dispozici ke stažení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="354f9-109">The following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="354f9-110">Vytvoření oboru názvů Service Bus</span><span class="sxs-lookup"><span data-stu-id="354f9-110">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="354f9-111">Vytvoření oboru názvů Service Bus pomocí fronty</span><span class="sxs-lookup"><span data-stu-id="354f9-111">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="354f9-112">Vytvoření oboru názvů Service Bus pomocí fronty a autorizační pravidla</span><span class="sxs-lookup"><span data-stu-id="354f9-112">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="354f9-113">Vytvoření oboru názvů Service Bus pomocí tématu, předplatné a pravidla</span><span class="sxs-lookup"><span data-stu-id="354f9-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="354f9-114">Vyhledat nejnovější šablony, najdete [šablon Azure rychlý Start] [ Azure Quickstart Templates] galerie a vyhledejte "Service Bus".</span><span class="sxs-lookup"><span data-stu-id="354f9-114">To check for the latest templates, visit the [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for "Service Bus."</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="354f9-115">Co budete nasazovat?</span><span class="sxs-lookup"><span data-stu-id="354f9-115">What will you deploy?</span></span>

<span data-ttu-id="354f9-116">Pomocí této šablony nasadíte v oboru názvů Service Bus s téma a odběr.</span><span class="sxs-lookup"><span data-stu-id="354f9-116">With this template, you will deploy a Service Bus namespace with topic and subscription.</span></span>

<span data-ttu-id="354f9-117">[Témata a odběry Service Bus](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) poskytovat ve formuláři na více komunikace, *publikování a přihlášení k odběru* vzor.</span><span class="sxs-lookup"><span data-stu-id="354f9-117">[Service Bus topics and subscriptions](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) provide a one-to-many form of communication, in a *publish/subscribe* pattern.</span></span>

<span data-ttu-id="354f9-118">Pokud chcete nasazení spustit automaticky, klikněte na následující tlačítko:</span><span class="sxs-lookup"><span data-stu-id="354f9-118">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="354f9-119">[![Nasazení do Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="354f9-119">[![Deploy to Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="354f9-120">Parametry</span><span class="sxs-lookup"><span data-stu-id="354f9-120">Parameters</span></span>

<span data-ttu-id="354f9-121">Pomocí Azure Resource Manageru definujete parametry pro hodnoty, které chcete zadat při nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="354f9-121">With Azure Resource Manager, you define parameters for values you want to specify when the template is deployed.</span></span> <span data-ttu-id="354f9-122">Šablona obsahuje oddíl s názvem `Parameters` obsahující všechny hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="354f9-122">The template includes a section called `Parameters` that contains all of the parameter values.</span></span> <span data-ttu-id="354f9-123">Měli byste parametr pro ty hodnoty, které se liší podle prostředí, ve kterém provádíte nasazení nebo na základě projektu, které nasazujete.</span><span class="sxs-lookup"><span data-stu-id="354f9-123">You should define a parameter for those values that will vary based on the project you are deploying or based on the environment you are deploying to.</span></span> <span data-ttu-id="354f9-124">Nedefinují parametry pro hodnoty, které zůstanou vždy stejná.</span><span class="sxs-lookup"><span data-stu-id="354f9-124">Do not define parameters for values that will always stay the same.</span></span> <span data-ttu-id="354f9-125">Každá hodnota parametru se v šabloně použije k definování nasazovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="354f9-125">Each parameter value is used in the template to define the resources that are deployed.</span></span>

<span data-ttu-id="354f9-126">Šablona definuje následující parametry.</span><span class="sxs-lookup"><span data-stu-id="354f9-126">The template defines the following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="354f9-127">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="354f9-127">serviceBusNamespaceName</span></span>
<span data-ttu-id="354f9-128">Název oboru názvů Service Bus k vytvoření.</span><span class="sxs-lookup"><span data-stu-id="354f9-128">The name of the Service Bus namespace to create.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a><span data-ttu-id="354f9-129">serviceBusTopicName</span><span class="sxs-lookup"><span data-stu-id="354f9-129">serviceBusTopicName</span></span>
<span data-ttu-id="354f9-130">Název tématu vytvořené v oboru názvů Service Bus.</span><span class="sxs-lookup"><span data-stu-id="354f9-130">The name of the topic created in the Service Bus namespace.</span></span>

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a><span data-ttu-id="354f9-131">serviceBusSubscriptionName</span><span class="sxs-lookup"><span data-stu-id="354f9-131">serviceBusSubscriptionName</span></span>
<span data-ttu-id="354f9-132">Název odběru vytvořené v oboru názvů Service Bus.</span><span class="sxs-lookup"><span data-stu-id="354f9-132">The name of the subscription created in the Service Bus namespace.</span></span>

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a><span data-ttu-id="354f9-133">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="354f9-133">serviceBusApiVersion</span></span>
<span data-ttu-id="354f9-134">Verze rozhraní API služby Service Bus šablony.</span><span class="sxs-lookup"><span data-stu-id="354f9-134">The Service Bus API version of the template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-to-deploy"></a><span data-ttu-id="354f9-135">Prostředky k nasazení</span><span class="sxs-lookup"><span data-stu-id="354f9-135">Resources to deploy</span></span>
<span data-ttu-id="354f9-136">Vytvoří standardní oboru názvů Service Bus typu **zasílání zpráv**, tématu a odběru.</span><span class="sxs-lookup"><span data-stu-id="354f9-136">Creates a standard Service Bus namespace of type **Messaging**, with topic and subscription.</span></span>

```json
"resources ": [{
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
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]",
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {}
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a><span data-ttu-id="354f9-137">Příkazy pro spuštění nasazení</span><span class="sxs-lookup"><span data-stu-id="354f9-137">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="354f9-138">PowerShell</span><span class="sxs-lookup"><span data-stu-id="354f9-138">PowerShell</span></span>
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="354f9-139">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="354f9-139">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="354f9-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="354f9-140">Next steps</span></span>
<span data-ttu-id="354f9-141">Teď, když jste vytvoření a nasazení prostředků pomocí Azure Resource Manager, zjistěte, jak tyto zdroje spravovat pomocí těchto článků:</span><span class="sxs-lookup"><span data-stu-id="354f9-141">Now that you've created and deployed resources using Azure Resource Manager, learn how to manage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="354f9-142">Správa služby Service Bus pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="354f9-142">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="354f9-143">Správa prostředků služby Service Bus pomocí Průzkumníka služby sběrnice</span><span class="sxs-lookup"><span data-stu-id="354f9-143">Manage Service Bus resources with the Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus namespace with topic and subscription]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-and-subscription/
