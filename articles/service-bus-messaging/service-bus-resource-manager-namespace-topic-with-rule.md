---
title: "aaaCreate odběr tématu Azure Service Bus a pravidla pomocí šablony Azure Resource Manager | Microsoft Docs"
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
ms.openlocfilehash: dbc46da8491aee4d0c73bd4db90c696008920df4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a><span data-ttu-id="d0792-103">Vytvoření oboru názvů Service Bus pomocí tématu, předplatné a pravidla pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="d0792-103">Create a Service Bus namespace with topic, subscription, and rule using an Azure Resource Manager template</span></span>

<span data-ttu-id="d0792-104">Tento článek ukazuje, jak toouse šablonu Azure Resource Manager, vytvoří obor názvů sběrnice se tématu, předplatné a pravidlo (filtr).</span><span class="sxs-lookup"><span data-stu-id="d0792-104">This article shows how toouse an Azure Resource Manager template that creates a Service Bus namespace with a topic, subscription, and rule (filter).</span></span> <span data-ttu-id="d0792-105">Zjistíte, jak toodefine prostředky, ke kterým jsou nasazené a jak toodefine parametry, které jsou zadané, když se spustí nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="d0792-105">You learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="d0792-106">Můžete tuto šablonu použít pro vlastní nasazení, nebo si ji přizpůsobit toomeet požadavků</span><span class="sxs-lookup"><span data-stu-id="d0792-106">You can use this template for your own deployments, or customize it toomeet your requirements</span></span>

<span data-ttu-id="d0792-107">Další informace o vytváření šablon najdete v tématu [Tvorba šablon Azure Resource Manageru][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="d0792-107">For more information about creating templates, see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="d0792-108">Další informace o postupů a vzorů na prostředky Azure konvence vytváření názvů najdete v tématu [doporučená zásady vytváření názvů pro prostředky Azure][Recommended naming conventions for Azure resources].</span><span class="sxs-lookup"><span data-stu-id="d0792-108">For more information about practice and patterns on Azure resources naming conventions, see [Recommended naming conventions for Azure resources][Recommended naming conventions for Azure resources].</span></span>

<span data-ttu-id="d0792-109">Hello úplnou šablonu, najdete v části hello [oboru názvů Service Bus s tématu, předplatné a pravidlo] [ Service Bus namespace with topic, subscription, and rule] šablony.</span><span class="sxs-lookup"><span data-stu-id="d0792-109">For hello complete template, see hello [Service Bus namespace with topic, subscription, and rule][Service Bus namespace with topic, subscription, and rule] template.</span></span>

> [!NOTE]
> <span data-ttu-id="d0792-110">Hello následující šablony Azure Resource Manager jsou k dispozici ke stažení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="d0792-110">hello following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="d0792-111">Vytvoření oboru názvů Service Bus pomocí fronty a autorizační pravidla</span><span class="sxs-lookup"><span data-stu-id="d0792-111">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="d0792-112">Vytvoření oboru názvů Service Bus pomocí fronty</span><span class="sxs-lookup"><span data-stu-id="d0792-112">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="d0792-113">Vytvoření oboru názvů Service Bus</span><span class="sxs-lookup"><span data-stu-id="d0792-113">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="d0792-114">Vytvoření oboru názvů Service Bus s téma a odběr</span><span class="sxs-lookup"><span data-stu-id="d0792-114">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
> 
> <span data-ttu-id="d0792-115">toocheck pro hello nejnovější šablony, navštivte hello [šablon Azure rychlý Start] [ Azure Quickstart Templates] galerie a vyhledejte Service Bus.</span><span class="sxs-lookup"><span data-stu-id="d0792-115">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Service Bus.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="d0792-116">Co budete nasazovat?</span><span class="sxs-lookup"><span data-stu-id="d0792-116">What will you deploy?</span></span>

<span data-ttu-id="d0792-117">Pomocí této šablony můžete nasadit v oboru názvů Service Bus s tématu, předplatné a pravidlo (filtr).</span><span class="sxs-lookup"><span data-stu-id="d0792-117">With this template, you deploy a Service Bus namespace with topic, subscription, and rule (filter).</span></span>

<span data-ttu-id="d0792-118">[Témata a odběry Service Bus](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) poskytovat ve formuláři na více komunikace, *publikování a přihlášení k odběru* vzor.</span><span class="sxs-lookup"><span data-stu-id="d0792-118">[Service Bus topics and subscriptions](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) provide a one-to-many form of communication, in a *publish/subscribe* pattern.</span></span> <span data-ttu-id="d0792-119">Při používání témat a odběrů, součásti distribuované aplikace nekomunikují navzájem přímo, místo toho si zprávy vyměňují prostřednictvím tématu, která slouží jako prostředník. Téma tooa předplatného se podobá virtuální frontě, která obdrží kopii zprávy, které byly odeslány toohello tématu.</span><span class="sxs-lookup"><span data-stu-id="d0792-119">When using topics and subscriptions, components of a distributed application do not communicate directly with each other, instead they exchange messages via topic that acts as an intermediary.A subscription tooa topic resembles a virtual queue that receives copies of messages that were sent toohello topic.</span></span> <span data-ttu-id="d0792-120">Filtr na předplatné vám umožní toospecify příjem zpráv odeslaných tooa tématu by měl být použit v konkrétním odběru tématu.</span><span class="sxs-lookup"><span data-stu-id="d0792-120">A filter on subscription enables you toospecify which messages sent tooa topic should appear within a specific topic subscription.</span></span>

## <a name="what-are-rules-filters"></a><span data-ttu-id="d0792-121">Co jsou pravidla (filtry)?</span><span class="sxs-lookup"><span data-stu-id="d0792-121">What are rules (filters)?</span></span>

<span data-ttu-id="d0792-122">V mnoha případech je nutné zpracovat zprávy, které mají určité charakteristické vlastnosti různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="d0792-122">In many scenarios, messages that have specific characteristics must be processed in different ways.</span></span> <span data-ttu-id="d0792-123">tooenable, můžete nakonfigurovat odběry toofind zprávy, které mají určité vlastnosti a poté proveďte úpravy toothose vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d0792-123">tooenable this, you can configure subscriptions toofind messages that have specific properties and then perform modifications toothose properties.</span></span> <span data-ttu-id="d0792-124">I když Service Bus odběry všech zpráv odeslaných toohello tématu najdete kopírovat můžete pouze podmnožinu těchto fronty zpráv toohello virtuální odběru.</span><span class="sxs-lookup"><span data-stu-id="d0792-124">Although Service Bus subscriptions see all messages sent toohello topic, you can only copy a subset of those messages toohello virtual subscription queue.</span></span> <span data-ttu-id="d0792-125">To se provádí pomocí filtrů odběrů.</span><span class="sxs-lookup"><span data-stu-id="d0792-125">This is accomplished using subscription filters.</span></span> <span data-ttu-id="d0792-126">toolearn Další informace o pravidlech (filtry), najdete v části [pravidla a akce](service-bus-queues-topics-subscriptions.md#rules-and-actions).</span><span class="sxs-lookup"><span data-stu-id="d0792-126">toolearn more about rules (filters), see [Rules and actions](service-bus-queues-topics-subscriptions.md#rules-and-actions).</span></span>

<span data-ttu-id="d0792-127">toorun hello nasazení automaticky, klikněte na následující tlačítko hello:</span><span class="sxs-lookup"><span data-stu-id="d0792-127">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="d0792-128">[![Nasazení tooAzure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="d0792-128">[![Deploy tooAzure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="d0792-129">Parametry</span><span class="sxs-lookup"><span data-stu-id="d0792-129">Parameters</span></span>

<span data-ttu-id="d0792-130">S Azure Resource Manager, byste měli definovat parametry pro hodnoty chcete toospecify při nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="d0792-130">With Azure Resource Manager, you should define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="d0792-131">Šablona Hello zahrnuje části s názvem `Parameters` obsahující všechny hodnoty parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="d0792-131">hello template includes a section called `Parameters` that contains all hello parameter values.</span></span> <span data-ttu-id="d0792-132">Měli byste parametr pro ty hodnoty, které se liší podle hello prostředí, které nasazujete nebo na základě hello projektu, které nasazujete.</span><span class="sxs-lookup"><span data-stu-id="d0792-132">You should define a parameter for those values that vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="d0792-133">Parametry nedefinuje pro hello hodnoty, které vždy zůstávají stejné.</span><span class="sxs-lookup"><span data-stu-id="d0792-133">Do not define parameters for values that always stay hello same.</span></span> <span data-ttu-id="d0792-134">Každá hodnota parametru se používá v hello šablony toodefine hello prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="d0792-134">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span>

<span data-ttu-id="d0792-135">Šablona Hello definuje hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="d0792-135">hello template defines hello following parameters:</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="d0792-136">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="d0792-136">serviceBusNamespaceName</span></span>
<span data-ttu-id="d0792-137">Název Hello toocreate oboru názvů Service Bus hello.</span><span class="sxs-lookup"><span data-stu-id="d0792-137">hello name of hello Service Bus namespace toocreate.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a><span data-ttu-id="d0792-138">serviceBusTopicName</span><span class="sxs-lookup"><span data-stu-id="d0792-138">serviceBusTopicName</span></span>
<span data-ttu-id="d0792-139">Hello název tématu hello vytvořené v oboru názvů Service Bus hello.</span><span class="sxs-lookup"><span data-stu-id="d0792-139">hello name of hello topic created in hello Service Bus namespace.</span></span>

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a><span data-ttu-id="d0792-140">serviceBusSubscriptionName</span><span class="sxs-lookup"><span data-stu-id="d0792-140">serviceBusSubscriptionName</span></span>
<span data-ttu-id="d0792-141">Hello název odběru hello vytvořené v oboru názvů Service Bus hello.</span><span class="sxs-lookup"><span data-stu-id="d0792-141">hello name of hello subscription created in hello Service Bus namespace.</span></span>

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```
### <a name="servicebusrulename"></a><span data-ttu-id="d0792-142">serviceBusRuleName</span><span class="sxs-lookup"><span data-stu-id="d0792-142">serviceBusRuleName</span></span>
<span data-ttu-id="d0792-143">Název Hello hello rule(filter) vytvořené v oboru názvů Service Bus hello.</span><span class="sxs-lookup"><span data-stu-id="d0792-143">hello name of hello rule(filter) created in hello Service Bus namespace.</span></span>

```json
   "serviceBusRuleName": {
   "type": "string",
  }
```
### <a name="servicebusapiversion"></a><span data-ttu-id="d0792-144">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="d0792-144">serviceBusApiVersion</span></span>
<span data-ttu-id="d0792-145">verze rozhraní API služby Service Bus Hello hello šablony.</span><span class="sxs-lookup"><span data-stu-id="d0792-145">hello Service Bus API version of hello template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-toodeploy"></a><span data-ttu-id="d0792-146">Toodeploy prostředky</span><span class="sxs-lookup"><span data-stu-id="d0792-146">Resources toodeploy</span></span>
<span data-ttu-id="d0792-147">Vytvoří standardní oboru názvů Service Bus typu **zasílání zpráv**, tématu a odběru a pravidla.</span><span class="sxs-lookup"><span data-stu-id="d0792-147">Creates a standard Service Bus namespace of type **Messaging**, with topic and subscription and rules.</span></span>

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

## <a name="commands-toorun-deployment"></a><span data-ttu-id="d0792-148">Příkazy toorun nasazení</span><span class="sxs-lookup"><span data-stu-id="d0792-148">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="d0792-149">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d0792-149">PowerShell</span></span>
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="d0792-150">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d0792-150">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="d0792-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d0792-151">Next steps</span></span>
<span data-ttu-id="d0792-152">Nyní, po vytvoření a nasazení prostředků pomocí Azure Resource Manager, se naučíte, jak toomanage tyto prostředky pomocí těchto článků:</span><span class="sxs-lookup"><span data-stu-id="d0792-152">Now that you've created and deployed resources using Azure Resource Manager, learn how toomanage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="d0792-153">Správa služby Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="d0792-153">Manage Azure Service Bus</span></span>](service-bus-management-libraries.md)
* [<span data-ttu-id="d0792-154">Správa služby Service Bus pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d0792-154">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="d0792-155">Správa prostředků služby Service Bus pomocí hello Service Bus Explorer</span><span class="sxs-lookup"><span data-stu-id="d0792-155">Manage Service Bus resources with hello Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Recommended naming conventions for Azure resources]: ../guidance/guidance-naming-conventions.md
[Service Bus namespace with topic, subscription, and rule]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md

