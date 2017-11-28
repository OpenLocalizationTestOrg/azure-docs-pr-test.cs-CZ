---
title: "aaaCreate Service Bus autorizační pravidlo pomocí šablony Azure Resource Manager | Microsoft Docs"
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
ms.openlocfilehash: 48df97849281d3b47e9d722d4e821c874644be59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a><span data-ttu-id="9b2e5-103">Vytvoření pravidla autorizace sběrnice pro obor názvů a fronty pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9b2e5-103">Create a Service Bus authorization rule for namespace and queue using an Azure Resource Manager template</span></span>

<span data-ttu-id="9b2e5-104">Tento článek ukazuje, jak toouse šablonu Azure Resource Manager vytvářející [autorizační pravidlo](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) oboru názvů Service Bus a fronty.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-104">This article shows how toouse an Azure Resource Manager template that creates an [authorization rule](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) for a Service Bus namespace and queue.</span></span> <span data-ttu-id="9b2e5-105">Se dozvíte, jak toodefine prostředky, ke kterým jsou nasazené a jak toodefine parametry, které jsou zadané, když se spustí nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-105">You will learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="9b2e5-106">Můžete tuto šablonu použít pro vlastní nasazení, nebo si ji přizpůsobit toomeet vašim požadavkům.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-106">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="9b2e5-107">Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="9b2e5-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="9b2e5-108">Hello úplnou šablonu, najdete v části hello [šablonu pravidla autorizace služby Service Bus] [ Service Bus auth rule template] na Githubu.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-108">For hello complete template, see hello [Service Bus authorization rule template][Service Bus auth rule template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="9b2e5-109">Hello následující šablony Azure Resource Manager jsou k dispozici ke stažení a nasazení.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-109">hello following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="9b2e5-110">Vytvoření oboru názvů Service Bus</span><span class="sxs-lookup"><span data-stu-id="9b2e5-110">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="9b2e5-111">Vytvoření oboru názvů Service Bus pomocí fronty</span><span class="sxs-lookup"><span data-stu-id="9b2e5-111">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="9b2e5-112">Vytvoření oboru názvů Service Bus s téma a odběr</span><span class="sxs-lookup"><span data-stu-id="9b2e5-112">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
> * [<span data-ttu-id="9b2e5-113">Vytvoření oboru názvů Service Bus pomocí tématu, předplatné a pravidla</span><span class="sxs-lookup"><span data-stu-id="9b2e5-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="9b2e5-114">toocheck pro hello nejnovější šablony, navštivte hello [šablon Azure rychlý Start] [ Azure Quickstart Templates] galerie a vyhledejte "Service Bus".</span><span class="sxs-lookup"><span data-stu-id="9b2e5-114">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for "Service Bus."</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="9b2e5-115">Co budete nasazovat?</span><span class="sxs-lookup"><span data-stu-id="9b2e5-115">What will you deploy?</span></span>
<span data-ttu-id="9b2e5-116">Pomocí této šablony nasadíte Service Bus autorizační pravidlo pro obor názvů a entity přenosu zpráv (v tomto případě fronty).</span><span class="sxs-lookup"><span data-stu-id="9b2e5-116">With this template, you will deploy a Service Bus authorization rule for a namespace and messaging entity (in this case, a queue).</span></span>

<span data-ttu-id="9b2e5-117">Tato šablona používá [sdíleného přístupového podpisu (SAS)](service-bus-sas.md) pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-117">This template uses [Shared Access Signature (SAS)](service-bus-sas.md) for authentication.</span></span> <span data-ttu-id="9b2e5-118">SAS umožňuje aplikacím tooauthenticate tooService sběrnice pomocí přístupový klíč nakonfigurované na oboru názvů hello nebo na hello zasílání zpráv entity (fronta nebo téma) pomocí konkrétní práva, která jsou přidružená.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-118">SAS enables applications tooauthenticate tooService Bus using an access key configured on hello namespace, or on hello messaging entity (queue or topic) with which specific rights are associated.</span></span> <span data-ttu-id="9b2e5-119">Pak můžete použít tento klíč toogenerate token SAS, který můžou klienti zase použít tooauthenticate tooService sběrnice.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-119">You can then use this key toogenerate a SAS token that clients can in turn use tooauthenticate tooService Bus.</span></span>

<span data-ttu-id="9b2e5-120">toorun hello nasazení automaticky, klikněte na následující tlačítko hello:</span><span class="sxs-lookup"><span data-stu-id="9b2e5-120">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="9b2e5-121">[![Nasazení tooAzure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="9b2e5-121">[![Deploy tooAzure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="9b2e5-122">Parametry</span><span class="sxs-lookup"><span data-stu-id="9b2e5-122">Parameters</span></span>

<span data-ttu-id="9b2e5-123">S Azure Resource Manager, můžete definovat parametry pro hodnoty chcete toospecify při nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-123">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="9b2e5-124">Šablona Hello zahrnuje části s názvem `Parameters` obsahující všechny hodnoty parametru hello.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-124">hello template includes a section called `Parameters` that contains all of hello parameter values.</span></span> <span data-ttu-id="9b2e5-125">Měli byste parametr pro ty hodnoty, které se liší podle hello prostředí, které nasazujete nebo na základě hello projektu, které nasazujete.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-125">You should define a parameter for those values that will vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="9b2e5-126">Parametry nedefinuje pro hodnoty, které vždy zůstanou hello stejné.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-126">Do not define parameters for values that will always stay hello same.</span></span> <span data-ttu-id="9b2e5-127">Každá hodnota parametru se používá v hello šablony toodefine hello prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-127">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span>

<span data-ttu-id="9b2e5-128">Šablona Hello definuje hello následující parametry.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-128">hello template defines hello following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="9b2e5-129">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="9b2e5-129">serviceBusNamespaceName</span></span>
<span data-ttu-id="9b2e5-130">Název Hello toocreate oboru názvů Service Bus hello.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-130">hello name of hello Service Bus namespace toocreate.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="namespaceauthorizationrulename"></a><span data-ttu-id="9b2e5-131">namespaceAuthorizationRuleName</span><span class="sxs-lookup"><span data-stu-id="9b2e5-131">namespaceAuthorizationRuleName</span></span>
<span data-ttu-id="9b2e5-132">Hello název hello autorizační pravidlo pro obor názvů hello.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-132">hello name of hello authorization rule for hello namespace.</span></span>

```json
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a><span data-ttu-id="9b2e5-133">serviceBusQueueName</span><span class="sxs-lookup"><span data-stu-id="9b2e5-133">serviceBusQueueName</span></span>
<span data-ttu-id="9b2e5-134">Název Hello hello fronty v oboru názvů Service Bus hello.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-134">hello name of hello queue in hello Service Bus namespace.</span></span>

```json
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a><span data-ttu-id="9b2e5-135">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="9b2e5-135">serviceBusApiVersion</span></span>
<span data-ttu-id="9b2e5-136">verze rozhraní API služby Service Bus Hello hello šablony.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-136">hello Service Bus API version of hello template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```

## <a name="resources-toodeploy"></a><span data-ttu-id="9b2e5-137">Toodeploy prostředky</span><span class="sxs-lookup"><span data-stu-id="9b2e5-137">Resources toodeploy</span></span>
<span data-ttu-id="9b2e5-138">Vytvoří standardní oboru názvů Service Bus typu **zasílání zpráv**a Service Bus autorizační pravidlo pro obor názvů a entity.</span><span class="sxs-lookup"><span data-stu-id="9b2e5-138">Creates a standard Service Bus namespace of type **Messaging**, and a Service Bus authorization rule for namespace and entity.</span></span>

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

## <a name="commands-toorun-deployment"></a><span data-ttu-id="9b2e5-139">Příkazy toorun nasazení</span><span class="sxs-lookup"><span data-stu-id="9b2e5-139">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="9b2e5-140">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9b2e5-140">PowerShell</span></span>
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="9b2e5-141">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9b2e5-141">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="9b2e5-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9b2e5-142">Next steps</span></span>
<span data-ttu-id="9b2e5-143">Nyní, po vytvoření a nasazení prostředků pomocí Azure Resource Manager, se naučíte, jak toomanage tyto prostředky pomocí těchto článků:</span><span class="sxs-lookup"><span data-stu-id="9b2e5-143">Now that you've created and deployed resources using Azure Resource Manager, learn how toomanage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="9b2e5-144">Správa služby Service Bus pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9b2e5-144">Manage Service Bus with PowerShell</span></span>](service-bus-powershell-how-to-provision.md)
* [<span data-ttu-id="9b2e5-145">Správa prostředků služby Service Bus pomocí hello Service Bus Explorer</span><span class="sxs-lookup"><span data-stu-id="9b2e5-145">Manage Service Bus resources with hello Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)
* [<span data-ttu-id="9b2e5-146">Service Bus ověřování a autorizace</span><span class="sxs-lookup"><span data-stu-id="9b2e5-146">Service Bus authentication and authorization</span></span>](service-bus-authentication-and-authorization.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus auth rule template]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
