---
title: "obor názvů aaaCreate Azure Service Bus a fronty pomocí šablony Azure Resource Manager | Microsoft Docs"
description: "Vytvoření oboru názvů Service Bus a fronty pomocí šablony Azure Resource Manageru"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a6bfb5fd-7b98-4588-8aa1-9d5f91b599b6
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: f230878b7c557bdd80d74da0de5a85ba4ee99ef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-bus-namespace-and-a-queue-using-an-azure-resource-manager-template"></a><span data-ttu-id="5d6d8-103">Vytvoření oboru názvů Service Bus a fronty pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="5d6d8-103">Create a Service Bus namespace and a queue using an Azure Resource Manager template</span></span>

<span data-ttu-id="5d6d8-104">Tento článek ukazuje, jak toouse šablonu Azure Resource Manager, který vytvořil oboru názvů Service Bus a fronty v daném oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="5d6d8-104">This article shows how toouse an Azure Resource Manager template that creates a Service Bus namespace and a queue within that namespace.</span></span> <span data-ttu-id="5d6d8-105">Se dozvíte, jak toodefine prostředky, ke kterým jsou nasazené a jak toodefine parametry, které jsou zadané, když se spustí nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="5d6d8-105">You will learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="5d6d8-106">Můžete tuto šablonu použít pro vlastní nasazení, nebo si ji přizpůsobit toomeet vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="5d6d8-106">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="5d6d8-107">Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="5d6d8-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="5d6d8-108">Hello úplnou šablonu, najdete v části hello [šablony obor názvů a fronty Service Bus] [ Service Bus namespace and queue template] na Githubu.</span><span class="sxs-lookup"><span data-stu-id="5d6d8-108">For hello complete template, see hello [Service Bus namespace and queue template][Service Bus namespace and queue template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="5d6d8-109">Hello následující šablony Azure Resource Manager jsou k dispozici ke stažení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="5d6d8-109">hello following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="5d6d8-110">Vytvoření oboru názvů Service Bus pomocí fronty a autorizační pravidla</span><span class="sxs-lookup"><span data-stu-id="5d6d8-110">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="5d6d8-111">Vytvoření oboru názvů Service Bus s téma a odběr</span><span class="sxs-lookup"><span data-stu-id="5d6d8-111">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
> * [<span data-ttu-id="5d6d8-112">Vytvoření oboru názvů Service Bus</span><span class="sxs-lookup"><span data-stu-id="5d6d8-112">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="5d6d8-113">Vytvoření oboru názvů Service Bus pomocí tématu, předplatné a pravidla</span><span class="sxs-lookup"><span data-stu-id="5d6d8-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="5d6d8-114">toocheck pro hello nejnovější šablony, navštivte hello [šablon Azure rychlý Start] [ Azure Quickstart Templates] galerie a vyhledejte "Service Bus".</span><span class="sxs-lookup"><span data-stu-id="5d6d8-114">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for "Service Bus."</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="5d6d8-115">Co budete nasazovat?</span><span class="sxs-lookup"><span data-stu-id="5d6d8-115">What will you deploy?</span></span>

<span data-ttu-id="5d6d8-116">Pomocí této šablony nasadíte oboru názvů Service Bus s frontou.</span><span class="sxs-lookup"><span data-stu-id="5d6d8-116">With this template, you will deploy a Service Bus namespace with a queue.</span></span>

<span data-ttu-id="5d6d8-117">[Fronty služby Service Bus](service-bus-queues-topics-subscriptions.md#queues) nabízejí First In tooone doručení zpráv první Out (FIFO) nebo další konkurenčních spotřebitelů.</span><span class="sxs-lookup"><span data-stu-id="5d6d8-117">[Service Bus queues](service-bus-queues-topics-subscriptions.md#queues) offer First In, First Out (FIFO) message delivery tooone or more competing consumers.</span></span>

<span data-ttu-id="5d6d8-118">toorun hello nasazení automaticky, klikněte na následující tlačítko hello:</span><span class="sxs-lookup"><span data-stu-id="5d6d8-118">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="5d6d8-119">[![Nasazení tooAzure](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="5d6d8-119">[![Deploy tooAzure](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="5d6d8-120">Parametry</span><span class="sxs-lookup"><span data-stu-id="5d6d8-120">Parameters</span></span>

<span data-ttu-id="5d6d8-121">S Azure Resource Manager, můžete definovat parametry pro hodnoty chcete toospecify při nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="5d6d8-121">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="5d6d8-122">Šablona Hello zahrnuje části s názvem `Parameters` obsahující všechny hodnoty parametru hello.</span><span class="sxs-lookup"><span data-stu-id="5d6d8-122">hello template includes a section called `Parameters` that contains all of hello parameter values.</span></span> <span data-ttu-id="5d6d8-123">Měli byste parametr pro ty hodnoty, které se liší podle hello prostředí, které nasazujete nebo na základě hello projektu, které nasazujete.</span><span class="sxs-lookup"><span data-stu-id="5d6d8-123">You should define a parameter for those values that will vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="5d6d8-124">Parametry nedefinuje pro hodnoty, které vždy zůstanou hello stejné.</span><span class="sxs-lookup"><span data-stu-id="5d6d8-124">Do not define parameters for values that will always stay hello same.</span></span> <span data-ttu-id="5d6d8-125">Každá hodnota parametru se používá v hello šablony toodefine hello prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="5d6d8-125">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span>

<span data-ttu-id="5d6d8-126">Šablona Hello definuje hello následující parametry.</span><span class="sxs-lookup"><span data-stu-id="5d6d8-126">hello template defines hello following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="5d6d8-127">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="5d6d8-127">serviceBusNamespaceName</span></span>
<span data-ttu-id="5d6d8-128">Název Hello toocreate oboru názvů Service Bus hello.</span><span class="sxs-lookup"><span data-stu-id="5d6d8-128">hello name of hello Service Bus namespace toocreate.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of hello Service Bus namespace" 
    }
}
```

### <a name="servicebusqueuename"></a><span data-ttu-id="5d6d8-129">serviceBusQueueName</span><span class="sxs-lookup"><span data-stu-id="5d6d8-129">serviceBusQueueName</span></span>
<span data-ttu-id="5d6d8-130">Hello název fronty hello vytvořené v oboru názvů Service Bus hello.</span><span class="sxs-lookup"><span data-stu-id="5d6d8-130">hello name of hello queue created in hello Service Bus namespace.</span></span>

```json
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a><span data-ttu-id="5d6d8-131">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="5d6d8-131">serviceBusApiVersion</span></span>
<span data-ttu-id="5d6d8-132">verze rozhraní API služby Service Bus Hello hello šablony.</span><span class="sxs-lookup"><span data-stu-id="5d6d8-132">hello Service Bus API version of hello template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```

## <a name="resources-toodeploy"></a><span data-ttu-id="5d6d8-133">Toodeploy prostředky</span><span class="sxs-lookup"><span data-stu-id="5d6d8-133">Resources toodeploy</span></span>
<span data-ttu-id="5d6d8-134">Vytvoří standardní oboru názvů Service Bus typu **zasílání zpráv**, s frontou.</span><span class="sxs-lookup"><span data-stu-id="5d6d8-134">Creates a standard Service Bus namespace of type **Messaging**, with a queue.</span></span>

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
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]",
            }
        }]
    }]
```

## <a name="commands-toorun-deployment"></a><span data-ttu-id="5d6d8-135">Příkazy toorun nasazení</span><span class="sxs-lookup"><span data-stu-id="5d6d8-135">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="5d6d8-136">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5d6d8-136">PowerShell</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="5d6d8-137">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5d6d8-137">Azure CLI</span></span>

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="5d6d8-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5d6d8-138">Next steps</span></span>
<span data-ttu-id="5d6d8-139">Nyní, po vytvoření a nasazení prostředků pomocí Azure Resource Manager, se naučíte, jak toomanage tyto prostředky pomocí těchto článků:</span><span class="sxs-lookup"><span data-stu-id="5d6d8-139">Now that you've created and deployed resources using Azure Resource Manager, learn how toomanage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="5d6d8-140">Správa služby Service Bus pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="5d6d8-140">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="5d6d8-141">Správa prostředků služby Service Bus pomocí hello Service Bus Explorer</span><span class="sxs-lookup"><span data-stu-id="5d6d8-141">Manage Service Bus resources with hello Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace and queue template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus queues]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
