---
title: "Vytvořit odběr tématu Azure Service Bus a pravidla pomocí šablony Azure Resource Manager | Microsoft Docs"
description: "Vytvoření oboru názvů Service Bus pomocí tématu, předplatné a pravidla pomocí šablony Azure Resource Manager"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 9e0aaf58-0214-4bca-bd00-d29c08f9b1bc
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: 35e67d86b42358c4ce28b41beae1ee8e1896e939
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a><span data-ttu-id="93fee-103">Vytvoření oboru názvů Service Bus pomocí tématu, předplatné a pravidla pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="93fee-103">Create a Service Bus namespace with topic, subscription, and rule using an Azure Resource Manager template</span></span>

<span data-ttu-id="93fee-104">Tento článek ukazuje, jak používat šablonu Azure Resource Manager, která vytvoří obor názvů sběrnice s téma, předplatné a pravidlo (filtr).</span><span class="sxs-lookup"><span data-stu-id="93fee-104">This article shows how to use an Azure Resource Manager template that creates a Service Bus namespace with a topic, subscription, and rule (filter).</span></span> <span data-ttu-id="93fee-105">Zjistíte, jak definovat prostředky, ke kterým nasazených a jak definovat parametry, které jsou zadané, když se spustí nasazení.</span><span class="sxs-lookup"><span data-stu-id="93fee-105">You learn how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="93fee-106">Tuto šablonu můžete použít pro vlastní nasazení nebo ji upravit, aby splňovala vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="93fee-106">You can use this template for your own deployments, or customize it to meet your requirements</span></span>

<span data-ttu-id="93fee-107">Další informace o vytváření šablon najdete v tématu [Tvorba šablon Azure Resource Manageru][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="93fee-107">For more information about creating templates, see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="93fee-108">Další informace o postupů a vzorů na prostředky Azure konvence vytváření názvů najdete v tématu [doporučená zásady vytváření názvů pro prostředky Azure][Recommended naming conventions for Azure resources].</span><span class="sxs-lookup"><span data-stu-id="93fee-108">For more information about practice and patterns on Azure resources naming conventions, see [Recommended naming conventions for Azure resources][Recommended naming conventions for Azure resources].</span></span>

<span data-ttu-id="93fee-109">Úplnou šablonu, najdete v článku [oboru názvů Service Bus s tématu, předplatné a pravidlo] [ Service Bus namespace with topic, subscription, and rule] šablony.</span><span class="sxs-lookup"><span data-stu-id="93fee-109">For the complete template, see the [Service Bus namespace with topic, subscription, and rule][Service Bus namespace with topic, subscription, and rule] template.</span></span>

> [!NOTE]
> <span data-ttu-id="93fee-110">Následující šablony Azure Resource Manager jsou k dispozici ke stažení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="93fee-110">The following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="93fee-111">Vytvoření oboru názvů Service Bus pomocí fronty a autorizační pravidla</span><span class="sxs-lookup"><span data-stu-id="93fee-111">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="93fee-112">Vytvoření oboru názvů Service Bus pomocí fronty</span><span class="sxs-lookup"><span data-stu-id="93fee-112">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="93fee-113">Vytvoření oboru názvů Service Bus</span><span class="sxs-lookup"><span data-stu-id="93fee-113">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="93fee-114">Vytvoření oboru názvů Service Bus s téma a odběr</span><span class="sxs-lookup"><span data-stu-id="93fee-114">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
> 
> <span data-ttu-id="93fee-115">Vyhledat nejnovější šablony, najdete [šablon Azure rychlý Start] [ Azure Quickstart Templates] galerie a vyhledejte Service Bus.</span><span class="sxs-lookup"><span data-stu-id="93fee-115">To check for the latest templates, visit the [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Service Bus.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="93fee-116">Co budete nasazovat?</span><span class="sxs-lookup"><span data-stu-id="93fee-116">What will you deploy?</span></span>

<span data-ttu-id="93fee-117">Pomocí této šablony můžete nasadit v oboru názvů Service Bus s tématu, předplatné a pravidlo (filtr).</span><span class="sxs-lookup"><span data-stu-id="93fee-117">With this template, you deploy a Service Bus namespace with topic, subscription, and rule (filter).</span></span>

<span data-ttu-id="93fee-118">[Témata a odběry Service Bus](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) poskytovat ve formuláři na více komunikace, *publikování a přihlášení k odběru* vzor.</span><span class="sxs-lookup"><span data-stu-id="93fee-118">[Service Bus topics and subscriptions](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) provide a one-to-many form of communication, in a *publish/subscribe* pattern.</span></span> <span data-ttu-id="93fee-119">Při používání témat a odběrů, součásti distribuované aplikace nekomunikují navzájem přímo, místo toho si zprávy vyměňují prostřednictvím tématu, která slouží jako prostředník. Předplatné tématu se podobá virtuální frontě, která obdrží kopii zprávy, které byly odeslány do tématu.</span><span class="sxs-lookup"><span data-stu-id="93fee-119">When using topics and subscriptions, components of a distributed application do not communicate directly with each other, instead they exchange messages via topic that acts as an intermediary.A subscription to a topic resembles a virtual queue that receives copies of messages that were sent to the topic.</span></span> <span data-ttu-id="93fee-120">Filtr na předplatné umožňuje vám určit, které zprávy odeslané do tématu by se zobrazit v konkrétním odběru tématu.</span><span class="sxs-lookup"><span data-stu-id="93fee-120">A filter on subscription enables you to specify which messages sent to a topic should appear within a specific topic subscription.</span></span>

## <a name="what-are-rules-filters"></a><span data-ttu-id="93fee-121">Co jsou pravidla (filtry)?</span><span class="sxs-lookup"><span data-stu-id="93fee-121">What are rules (filters)?</span></span>

<span data-ttu-id="93fee-122">V mnoha případech je nutné zpracovat zprávy, které mají určité charakteristické vlastnosti různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="93fee-122">In many scenarios, messages that have specific characteristics must be processed in different ways.</span></span> <span data-ttu-id="93fee-123">Chcete-li povolit, můžete nakonfigurovat odběry a vyhledat zprávy, které mají určité vlastnosti a poté proveďte úpravy, aby se tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="93fee-123">To enable this, you can configure subscriptions to find messages that have specific properties and then perform modifications to those properties.</span></span> <span data-ttu-id="93fee-124">I když odběry služby Service Bus zobrazit všechny zprávy odeslané do tématu, kopírovat můžete pouze podmnožinu těchto zpráv do fronty virtuální odběru.</span><span class="sxs-lookup"><span data-stu-id="93fee-124">Although Service Bus subscriptions see all messages sent to the topic, you can only copy a subset of those messages to the virtual subscription queue.</span></span> <span data-ttu-id="93fee-125">To se provádí pomocí filtrů odběrů.</span><span class="sxs-lookup"><span data-stu-id="93fee-125">This is accomplished using subscription filters.</span></span> <span data-ttu-id="93fee-126">Další informace o pravidlech (filtry) najdete v tématu [pravidla a akce](service-bus-queues-topics-subscriptions.md#rules-and-actions).</span><span class="sxs-lookup"><span data-stu-id="93fee-126">To learn more about rules (filters), see [Rules and actions](service-bus-queues-topics-subscriptions.md#rules-and-actions).</span></span>

<span data-ttu-id="93fee-127">Pokud chcete nasazení spustit automaticky, klikněte na následující tlačítko:</span><span class="sxs-lookup"><span data-stu-id="93fee-127">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="93fee-128">[![Nasazení do Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="93fee-128">[![Deploy to Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="93fee-129">Parametry</span><span class="sxs-lookup"><span data-stu-id="93fee-129">Parameters</span></span>

<span data-ttu-id="93fee-130">S Azure Resource Manager, byste měli definovat parametry pro hodnoty, které chcete zadat při nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="93fee-130">With Azure Resource Manager, you should define parameters for values you want to specify when the template is deployed.</span></span> <span data-ttu-id="93fee-131">Šablona obsahuje část `Parameters`, která obsahuje všechny hodnoty parametrů.</span><span class="sxs-lookup"><span data-stu-id="93fee-131">The template includes a section called `Parameters` that contains all the parameter values.</span></span> <span data-ttu-id="93fee-132">Parametr byste měli definovat pro hodnoty, které se mění v závislosti na nasazovaném projektu nebo prostředí, do kterého nasazujete.</span><span class="sxs-lookup"><span data-stu-id="93fee-132">You should define a parameter for those values that vary based on the project you are deploying or based on the environment you are deploying to.</span></span> <span data-ttu-id="93fee-133">Nedefinujte parametry pro hodnoty, které jsou vždy stejné.</span><span class="sxs-lookup"><span data-stu-id="93fee-133">Do not define parameters for values that always stay the same.</span></span> <span data-ttu-id="93fee-134">Každá hodnota parametru se v šabloně použije k definování nasazovaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="93fee-134">Each parameter value is used in the template to define the resources that are deployed.</span></span>

<span data-ttu-id="93fee-135">Šablona definuje následující parametry:</span><span class="sxs-lookup"><span data-stu-id="93fee-135">The template defines the following parameters:</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="93fee-136">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="93fee-136">serviceBusNamespaceName</span></span>
<span data-ttu-id="93fee-137">Název oboru názvů Service Bus k vytvoření.</span><span class="sxs-lookup"><span data-stu-id="93fee-137">The name of the Service Bus namespace to create.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a><span data-ttu-id="93fee-138">serviceBusTopicName</span><span class="sxs-lookup"><span data-stu-id="93fee-138">serviceBusTopicName</span></span>
<span data-ttu-id="93fee-139">Název tématu vytvořené v oboru názvů Service Bus.</span><span class="sxs-lookup"><span data-stu-id="93fee-139">The name of the topic created in the Service Bus namespace.</span></span>

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a><span data-ttu-id="93fee-140">serviceBusSubscriptionName</span><span class="sxs-lookup"><span data-stu-id="93fee-140">serviceBusSubscriptionName</span></span>
<span data-ttu-id="93fee-141">Název odběru vytvořené v oboru názvů Service Bus.</span><span class="sxs-lookup"><span data-stu-id="93fee-141">The name of the subscription created in the Service Bus namespace.</span></span>

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```
### <a name="servicebusrulename"></a><span data-ttu-id="93fee-142">serviceBusRuleName</span><span class="sxs-lookup"><span data-stu-id="93fee-142">serviceBusRuleName</span></span>
<span data-ttu-id="93fee-143">Název rule(filter) vytvořené v oboru názvů Service Bus.</span><span class="sxs-lookup"><span data-stu-id="93fee-143">The name of the rule(filter) created in the Service Bus namespace.</span></span>

```json
   "serviceBusRuleName": {
   "type": "string",
  }
```
### <a name="servicebusapiversion"></a><span data-ttu-id="93fee-144">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="93fee-144">serviceBusApiVersion</span></span>
<span data-ttu-id="93fee-145">Verze rozhraní API služby Service Bus šablony.</span><span class="sxs-lookup"><span data-stu-id="93fee-145">The Service Bus API version of the template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-to-deploy"></a><span data-ttu-id="93fee-146">Prostředky k nasazení</span><span class="sxs-lookup"><span data-stu-id="93fee-146">Resources to deploy</span></span>
<span data-ttu-id="93fee-147">Vytvoří standardní oboru názvů Service Bus typu **zasílání zpráv**, tématu a odběru a pravidla.</span><span class="sxs-lookup"><span data-stu-id="93fee-147">Creates a standard Service Bus namespace of type **Messaging**, with topic and subscription and rules.</span></span>

```json
 "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "sku": {
            "name": "Standard",
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
                "path": "[parameters('serviceBusTopicName')]"
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {},
                "resources": [{
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusRuleName')]",
                    "type": "Rules",
                    "dependsOn": [
                        "[parameters('serviceBusSubscriptionName')]"
                    ],
                    "properties": {
                        "filter": {
                            "sqlExpression": "StoreName = 'Store1'"
                        },
                        "action": {
                            "sqlExpression": "set FilterTag = 'true'"
                        }
                    }
                }]
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a><span data-ttu-id="93fee-148">Příkazy pro spuštění nasazení</span><span class="sxs-lookup"><span data-stu-id="93fee-148">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="93fee-149">PowerShell</span><span class="sxs-lookup"><span data-stu-id="93fee-149">PowerShell</span></span>
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="93fee-150">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="93fee-150">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="93fee-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="93fee-151">Next steps</span></span>
<span data-ttu-id="93fee-152">Teď, když jste vytvoření a nasazení prostředků pomocí Azure Resource Manager, zjistěte, jak tyto zdroje spravovat pomocí těchto článků:</span><span class="sxs-lookup"><span data-stu-id="93fee-152">Now that you've created and deployed resources using Azure Resource Manager, learn how to manage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="93fee-153">Správa služby Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="93fee-153">Manage Azure Service Bus</span></span>](service-bus-management-libraries.md)
* [<span data-ttu-id="93fee-154">Správa služby Service Bus pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="93fee-154">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="93fee-155">Správa prostředků služby Service Bus pomocí Průzkumníka služby sběrnice</span><span class="sxs-lookup"><span data-stu-id="93fee-155">Manage Service Bus resources with the Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Recommended naming conventions for Azure resources]: ../guidance/guidance-naming-conventions.md
[Service Bus namespace with topic, subscription, and rule]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md

