---
title: "aaaCreate Azure Event Hubs obor názvů a příjemce skupinu pomocí šablony | Microsoft Docs"
description: "Vytvoření oboru názvů Event Hubs s centra událostí a skupiny příjemců pomocí šablon Azure Resource Manageru"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 28bb4591-1fd7-444f-a327-4e67e8878798
ms.service: event-hubs
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/12/2017
ms.author: sethm;shvija
ms.openlocfilehash: 74b0d6b3fbe848705e2c20e628aa4e5269b53edb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a><span data-ttu-id="0cbb1-103">Vytvoření oboru názvů Event Hubs s skupina rozbočovače a příjemce událostí pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0cbb1-103">Create an Event Hubs namespace with event hub and consumer group using an Azure Resource Manager template</span></span>

<span data-ttu-id="0cbb1-104">Tento článek ukazuje, jak toouse šablonu Azure Resource Manager, vytvoří obor názvů typu služby Event Hubs s rozbočovači jedna událost a jedné skupiny uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0cbb1-104">This article shows how toouse an Azure Resource Manager template that creates a namespace of type Event Hubs, with one event hub and one consumer group.</span></span> <span data-ttu-id="0cbb1-105">Hello článek ukazuje, jak toodefine prostředky, ke kterým jsou nasazené a jak toodefine parametry, které jsou zadané, když se spustí nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="0cbb1-105">hello article shows how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="0cbb1-106">Můžete tuto šablonu použít pro vlastní nasazení, nebo si ji přizpůsobit toomeet požadavků</span><span class="sxs-lookup"><span data-stu-id="0cbb1-106">You can use this template for your own deployments, or customize it toomeet your requirements</span></span>

<span data-ttu-id="0cbb1-107">Další informace o vytváření šablon najdete v tématu [Tvorba šablon Azure Resource Manageru][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="0cbb1-107">For more information about creating templates, see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="0cbb1-108">Hello úplnou šablonu, najdete v části hello [šablony události rozbočovače a příjemce skupiny] [ Event Hub and consumer group template] na Githubu.</span><span class="sxs-lookup"><span data-stu-id="0cbb1-108">For hello complete template, see hello [Event hub and consumer group template][Event Hub and consumer group template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="0cbb1-109">toocheck pro hello nejnovější šablony, navštivte hello [šablon Azure rychlý Start] [ Azure Quickstart Templates] galerie a vyhledejte Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="0cbb1-109">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Event Hubs.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="0cbb1-110">Co budete nasazovat?</span><span class="sxs-lookup"><span data-stu-id="0cbb1-110">What will you deploy?</span></span>
<span data-ttu-id="0cbb1-111">Pomocí této šablony nasadíte na obor názvů služby Event Hubs s centra událostí a skupiny příjemců.</span><span class="sxs-lookup"><span data-stu-id="0cbb1-111">With this template, you will deploy an Event Hubs namespace with an event hub and a consumer group.</span></span>

<span data-ttu-id="0cbb1-112">[Služba Event Hubs](event-hubs-what-is-event-hubs.md) zpracovává používanou tooprovide událostí a telemetrie příjem příchozích dat tooAzure v masivním měřítku, s nízkou latencí a vysokou spolehlivostí události.</span><span class="sxs-lookup"><span data-stu-id="0cbb1-112">[Event Hubs](event-hubs-what-is-event-hubs.md) is an event processing service used tooprovide event and telemetry ingress tooAzure at massive scale, with low latency and high reliability.</span></span>

<span data-ttu-id="0cbb1-113">toorun hello nasazení automaticky, klikněte na následující tlačítko hello:</span><span class="sxs-lookup"><span data-stu-id="0cbb1-113">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="0cbb1-114">[![Nasazení tooAzure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="0cbb1-114">[![Deploy tooAzure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="0cbb1-115">Parametry</span><span class="sxs-lookup"><span data-stu-id="0cbb1-115">Parameters</span></span>
<span data-ttu-id="0cbb1-116">S Azure Resource Manager, můžete definovat parametry pro hodnoty chcete toospecify při nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="0cbb1-116">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="0cbb1-117">Šablona Hello zahrnuje části s názvem `Parameters` obsahující všechny hodnoty parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="0cbb1-117">hello template includes a section called `Parameters` that contains all hello parameter values.</span></span> <span data-ttu-id="0cbb1-118">Měli byste parametr pro ty hodnoty, které se budou lišit podle toowhich hello prostředí, které nasazujete nebo na základě hello projektu, které nasazujete.</span><span class="sxs-lookup"><span data-stu-id="0cbb1-118">You should define a parameter for those values that will vary, based on hello project you are deploying or based on hello environment toowhich you are deploying.</span></span> <span data-ttu-id="0cbb1-119">Parametry nedefinuje pro hello hodnoty, které vždy zůstávají stejné.</span><span class="sxs-lookup"><span data-stu-id="0cbb1-119">Do not define parameters for values that always stay hello same.</span></span> <span data-ttu-id="0cbb1-120">Každá hodnota parametru v šabloně hello definuje hello prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="0cbb1-120">Each parameter value in hello template defines hello resources that are deployed.</span></span>

<span data-ttu-id="0cbb1-121">Šablona Hello definuje hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="0cbb1-121">hello template defines hello following parameters:</span></span>

### <a name="eventhubnamespacename"></a><span data-ttu-id="0cbb1-122">eventHubNamespaceName</span><span class="sxs-lookup"><span data-stu-id="0cbb1-122">eventHubNamespaceName</span></span>
<span data-ttu-id="0cbb1-123">Název Hello toocreate oboru názvů služby Event Hubs hello.</span><span class="sxs-lookup"><span data-stu-id="0cbb1-123">hello name of hello Event Hubs namespace toocreate.</span></span>

```json
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a><span data-ttu-id="0cbb1-124">eventHubName</span><span class="sxs-lookup"><span data-stu-id="0cbb1-124">eventHubName</span></span>
<span data-ttu-id="0cbb1-125">Hello název centra událostí hello vytvořené v oboru názvů služby Event Hubs hello.</span><span class="sxs-lookup"><span data-stu-id="0cbb1-125">hello name of hello event hub created in hello Event Hubs namespace.</span></span>

```json
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a><span data-ttu-id="0cbb1-126">eventHubConsumerGroupName</span><span class="sxs-lookup"><span data-stu-id="0cbb1-126">eventHubConsumerGroupName</span></span>
<span data-ttu-id="0cbb1-127">Hello název skupiny příjemců hello vytvořené pro centra událostí hello.</span><span class="sxs-lookup"><span data-stu-id="0cbb1-127">hello name of hello consumer group created for hello event hub.</span></span>

```json
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a><span data-ttu-id="0cbb1-128">apiVersion</span><span class="sxs-lookup"><span data-stu-id="0cbb1-128">apiVersion</span></span>
<span data-ttu-id="0cbb1-129">Hello rozhraní API verze hello šablony.</span><span class="sxs-lookup"><span data-stu-id="0cbb1-129">hello API version of hello template.</span></span>

```json
"apiVersion": {
"type": "string"
}
```

## <a name="resources-toodeploy"></a><span data-ttu-id="0cbb1-130">Toodeploy prostředky</span><span class="sxs-lookup"><span data-stu-id="0cbb1-130">Resources toodeploy</span></span>
<span data-ttu-id="0cbb1-131">Vytvoří obor názvů typu **EventHubs**, s centra událostí a skupiny příjemců.</span><span class="sxs-lookup"><span data-stu-id="0cbb1-131">Creates a namespace of type **EventHubs**, with an event hub and a consumer group.</span></span>

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-toorun-deployment"></a><span data-ttu-id="0cbb1-132">Příkazy toorun nasazení</span><span class="sxs-lookup"><span data-stu-id="0cbb1-132">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="0cbb1-133">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0cbb1-133">PowerShell</span></span>
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a><span data-ttu-id="0cbb1-134">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0cbb1-134">Azure CLI</span></span>
```cli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

## <a name="next-steps"></a><span data-ttu-id="0cbb1-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0cbb1-135">Next steps</span></span>
<span data-ttu-id="0cbb1-136">Další informace o službě Event Hubs návštěvou hello následující odkazy:</span><span class="sxs-lookup"><span data-stu-id="0cbb1-136">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="0cbb1-137">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="0cbb1-137">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="0cbb1-138">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="0cbb1-138">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="0cbb1-139">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="0cbb1-139">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
