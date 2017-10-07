---
title: "aaaCreate Azure Service Bus obor názvů odběr tématu pomocí šablony Azure Resource Manager | Microsoft Docs"
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
ms.openlocfilehash: 9b5f7d8710e598b73c0a7ea3daf8c300f7fa9ecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-bus-namespace-with-topic-and-subscription-using-an-azure-resource-manager-template"></a><span data-ttu-id="be774-103">Vytvoření oboru názvů Service Bus s téma a odběr pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="be774-103">Create a Service Bus namespace with topic and subscription using an Azure Resource Manager template</span></span>

<span data-ttu-id="be774-104">Tento článek ukazuje, jak toouse šablonu Azure Resource Manager, vytvoří obor názvů sběrnice a téma a odběr v daném oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="be774-104">This article shows how toouse an Azure Resource Manager template that creates a Service Bus namespace and a topic and subscription within that namespace.</span></span> <span data-ttu-id="be774-105">Se dozvíte, jak toodefine prostředky, ke kterým jsou nasazené a jak toodefine parametry, které jsou zadané, když se spustí nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="be774-105">You will learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="be774-106">Můžete tuto šablonu použít pro vlastní nasazení, nebo si ji přizpůsobit toomeet požadavků</span><span class="sxs-lookup"><span data-stu-id="be774-106">You can use this template for your own deployments, or customize it toomeet your requirements</span></span>

<span data-ttu-id="be774-107">Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="be774-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="be774-108">Hello úplnou šablonu, najdete v části hello [oboru názvů Service Bus s téma a odběr] [ Service Bus namespace with topic and subscription] šablony.</span><span class="sxs-lookup"><span data-stu-id="be774-108">For hello complete template, see hello [Service Bus namespace with topic and subscription][Service Bus namespace with topic and subscription] template.</span></span>

> [!NOTE]
> <span data-ttu-id="be774-109">Hello následující šablony Azure Resource Manager jsou k dispozici ke stažení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="be774-109">hello following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="be774-110">Vytvoření oboru názvů Service Bus</span><span class="sxs-lookup"><span data-stu-id="be774-110">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="be774-111">Vytvoření oboru názvů Service Bus pomocí fronty</span><span class="sxs-lookup"><span data-stu-id="be774-111">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="be774-112">Vytvoření oboru názvů Service Bus pomocí fronty a autorizační pravidla</span><span class="sxs-lookup"><span data-stu-id="be774-112">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="be774-113">Vytvoření oboru názvů Service Bus pomocí tématu, předplatné a pravidla</span><span class="sxs-lookup"><span data-stu-id="be774-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="be774-114">toocheck pro hello nejnovější šablony, navštivte hello [šablon Azure rychlý Start] [ Azure Quickstart Templates] galerie a vyhledejte "Service Bus".</span><span class="sxs-lookup"><span data-stu-id="be774-114">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for "Service Bus."</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="be774-115">Co budete nasazovat?</span><span class="sxs-lookup"><span data-stu-id="be774-115">What will you deploy?</span></span>

<span data-ttu-id="be774-116">Pomocí této šablony nasadíte v oboru názvů Service Bus s téma a odběr.</span><span class="sxs-lookup"><span data-stu-id="be774-116">With this template, you will deploy a Service Bus namespace with topic and subscription.</span></span>

<span data-ttu-id="be774-117">[Témata a odběry Service Bus](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) poskytovat ve formuláři na více komunikace, *publikování a přihlášení k odběru* vzor.</span><span class="sxs-lookup"><span data-stu-id="be774-117">[Service Bus topics and subscriptions](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) provide a one-to-many form of communication, in a *publish/subscribe* pattern.</span></span>

<span data-ttu-id="be774-118">toorun hello nasazení automaticky, klikněte na následující tlačítko hello:</span><span class="sxs-lookup"><span data-stu-id="be774-118">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="be774-119">[![Nasazení tooAzure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="be774-119">[![Deploy tooAzure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="be774-120">Parametry</span><span class="sxs-lookup"><span data-stu-id="be774-120">Parameters</span></span>

<span data-ttu-id="be774-121">S Azure Resource Manager, můžete definovat parametry pro hodnoty chcete toospecify při nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="be774-121">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="be774-122">Šablona Hello zahrnuje části s názvem `Parameters` obsahující všechny hodnoty parametru hello.</span><span class="sxs-lookup"><span data-stu-id="be774-122">hello template includes a section called `Parameters` that contains all of hello parameter values.</span></span> <span data-ttu-id="be774-123">Měli byste parametr pro ty hodnoty, které se liší podle hello prostředí, které nasazujete nebo na základě hello projektu, které nasazujete.</span><span class="sxs-lookup"><span data-stu-id="be774-123">You should define a parameter for those values that will vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="be774-124">Parametry nedefinuje pro hodnoty, které vždy zůstanou hello stejné.</span><span class="sxs-lookup"><span data-stu-id="be774-124">Do not define parameters for values that will always stay hello same.</span></span> <span data-ttu-id="be774-125">Každá hodnota parametru se používá v hello šablony toodefine hello prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="be774-125">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span>

<span data-ttu-id="be774-126">Šablona Hello definuje hello následující parametry.</span><span class="sxs-lookup"><span data-stu-id="be774-126">hello template defines hello following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="be774-127">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="be774-127">serviceBusNamespaceName</span></span>
<span data-ttu-id="be774-128">Název Hello toocreate oboru názvů Service Bus hello.</span><span class="sxs-lookup"><span data-stu-id="be774-128">hello name of hello Service Bus namespace toocreate.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a><span data-ttu-id="be774-129">serviceBusTopicName</span><span class="sxs-lookup"><span data-stu-id="be774-129">serviceBusTopicName</span></span>
<span data-ttu-id="be774-130">Hello název tématu hello vytvořené v oboru názvů Service Bus hello.</span><span class="sxs-lookup"><span data-stu-id="be774-130">hello name of hello topic created in hello Service Bus namespace.</span></span>

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a><span data-ttu-id="be774-131">serviceBusSubscriptionName</span><span class="sxs-lookup"><span data-stu-id="be774-131">serviceBusSubscriptionName</span></span>
<span data-ttu-id="be774-132">Hello název odběru hello vytvořené v oboru názvů Service Bus hello.</span><span class="sxs-lookup"><span data-stu-id="be774-132">hello name of hello subscription created in hello Service Bus namespace.</span></span>

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a><span data-ttu-id="be774-133">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="be774-133">serviceBusApiVersion</span></span>
<span data-ttu-id="be774-134">verze rozhraní API služby Service Bus Hello hello šablony.</span><span class="sxs-lookup"><span data-stu-id="be774-134">hello Service Bus API version of hello template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-toodeploy"></a><span data-ttu-id="be774-135">Toodeploy prostředky</span><span class="sxs-lookup"><span data-stu-id="be774-135">Resources toodeploy</span></span>
<span data-ttu-id="be774-136">Vytvoří standardní oboru názvů Service Bus typu **zasílání zpráv**, tématu a odběru.</span><span class="sxs-lookup"><span data-stu-id="be774-136">Creates a standard Service Bus namespace of type **Messaging**, with topic and subscription.</span></span>

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

## <a name="commands-toorun-deployment"></a><span data-ttu-id="be774-137">Příkazy toorun nasazení</span><span class="sxs-lookup"><span data-stu-id="be774-137">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="be774-138">PowerShell</span><span class="sxs-lookup"><span data-stu-id="be774-138">PowerShell</span></span>
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="be774-139">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="be774-139">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="be774-140">Další kroky</span><span class="sxs-lookup"><span data-stu-id="be774-140">Next steps</span></span>
<span data-ttu-id="be774-141">Nyní, po vytvoření a nasazení prostředků pomocí Azure Resource Manager, se naučíte, jak toomanage tyto prostředky pomocí těchto článků:</span><span class="sxs-lookup"><span data-stu-id="be774-141">Now that you've created and deployed resources using Azure Resource Manager, learn how toomanage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="be774-142">Správa služby Service Bus pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="be774-142">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="be774-143">Správa prostředků služby Service Bus pomocí hello Service Bus Explorer</span><span class="sxs-lookup"><span data-stu-id="be774-143">Manage Service Bus resources with hello Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus namespace with topic and subscription]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-and-subscription/
