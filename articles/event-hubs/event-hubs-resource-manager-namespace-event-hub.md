---
title: "Vytvoření oboru názvů a příjemce skupiny Azure Event Hubs pomocí šablony | Microsoft Docs"
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
ms.openlocfilehash: eb9a80eec0326aaa605cb8b21aecbaeec94ff212
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a><span data-ttu-id="73ea3-103">Vytvoření oboru názvů Event Hubs s skupina rozbočovače a příjemce událostí pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="73ea3-103">Create an Event Hubs namespace with event hub and consumer group using an Azure Resource Manager template</span></span>

<span data-ttu-id="73ea3-104">Tento článek ukazuje, jak používat šablonu Azure Resource Manager, která vytvoří obor názvů typu Event Hubs s rozbočovači jedna událost a jedna skupina příjemců.</span><span class="sxs-lookup"><span data-stu-id="73ea3-104">This article shows how to use an Azure Resource Manager template that creates a namespace of type Event Hubs, with one event hub and one consumer group.</span></span> <span data-ttu-id="73ea3-105">Článek popisuje, jak definovat jsou nasazené prostředky, ke kterým a jak definovat parametry, které jsou zadané, když se spustí nasazení.</span><span class="sxs-lookup"><span data-stu-id="73ea3-105">The article shows how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="73ea3-106">Tuto šablonu můžete použít pro vlastní nasazení nebo ji upravit, aby splňovala vaše požadavky.</span><span class="sxs-lookup"><span data-stu-id="73ea3-106">You can use this template for your own deployments, or customize it to meet your requirements</span></span>

<span data-ttu-id="73ea3-107">Další informace o vytváření šablon najdete v tématu [Tvorba šablon Azure Resource Manageru][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="73ea3-107">For more information about creating templates, see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="73ea3-108">Úplnou šablonu, najdete v článku [šablony události rozbočovače a příjemce skupiny] [ Event Hub and consumer group template] na Githubu.</span><span class="sxs-lookup"><span data-stu-id="73ea3-108">For the complete template, see the [Event hub and consumer group template][Event Hub and consumer group template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="73ea3-109">Nejnovější šablony můžete zkontrolovat tak, že přejdete do galerie [Šablony Azure pro rychlý start][Azure Quickstart Templates] a vyhledáte Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="73ea3-109">To check for the latest templates, visit the [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Event Hubs.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="73ea3-110">Co budete nasazovat?</span><span class="sxs-lookup"><span data-stu-id="73ea3-110">What will you deploy?</span></span>
<span data-ttu-id="73ea3-111">Pomocí této šablony nasadíte na obor názvů služby Event Hubs s centra událostí a skupiny příjemců.</span><span class="sxs-lookup"><span data-stu-id="73ea3-111">With this template, you will deploy an Event Hubs namespace with an event hub and a consumer group.</span></span>

<span data-ttu-id="73ea3-112">Služba [Event Hubs](event-hubs-what-is-event-hubs.md) zpracovává události a zajišťuje příjem příchozích dat událostí a telemetrie do Azure v masivním měřítku, s nízkou latencí a vysokou spolehlivostí.</span><span class="sxs-lookup"><span data-stu-id="73ea3-112">[Event Hubs](event-hubs-what-is-event-hubs.md) is an event processing service used to provide event and telemetry ingress to Azure at massive scale, with low latency and high reliability.</span></span>

<span data-ttu-id="73ea3-113">Pokud chcete nasazení spustit automaticky, klikněte na následující tlačítko:</span><span class="sxs-lookup"><span data-stu-id="73ea3-113">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="73ea3-114">[![Nasazení do Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="73ea3-114">[![Deploy to Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="73ea3-115">Parametry</span><span class="sxs-lookup"><span data-stu-id="73ea3-115">Parameters</span></span>
<span data-ttu-id="73ea3-116">Pomocí Azure Resource Manageru definujete parametry pro hodnoty, které chcete zadat při nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="73ea3-116">With Azure Resource Manager, you define parameters for values you want to specify when the template is deployed.</span></span> <span data-ttu-id="73ea3-117">Šablona obsahuje část `Parameters`, která obsahuje všechny hodnoty parametrů.</span><span class="sxs-lookup"><span data-stu-id="73ea3-117">The template includes a section called `Parameters` that contains all the parameter values.</span></span> <span data-ttu-id="73ea3-118">Měli byste parametr pro ty hodnoty, které se budou lišit podle prostředí, do které nasazujete nebo na základě projektu, které nasazujete.</span><span class="sxs-lookup"><span data-stu-id="73ea3-118">You should define a parameter for those values that will vary, based on the project you are deploying or based on the environment to which you are deploying.</span></span> <span data-ttu-id="73ea3-119">Nedefinujte parametry pro hodnoty, které jsou vždy stejné.</span><span class="sxs-lookup"><span data-stu-id="73ea3-119">Do not define parameters for values that always stay the same.</span></span> <span data-ttu-id="73ea3-120">Každá hodnota parametru v šabloně definuje prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="73ea3-120">Each parameter value in the template defines the resources that are deployed.</span></span>

<span data-ttu-id="73ea3-121">Šablona definuje následující parametry:</span><span class="sxs-lookup"><span data-stu-id="73ea3-121">The template defines the following parameters:</span></span>

### <a name="eventhubnamespacename"></a><span data-ttu-id="73ea3-122">eventHubNamespaceName</span><span class="sxs-lookup"><span data-stu-id="73ea3-122">eventHubNamespaceName</span></span>
<span data-ttu-id="73ea3-123">Název oboru názvů Event Hubs, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="73ea3-123">The name of the Event Hubs namespace to create.</span></span>

```json
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a><span data-ttu-id="73ea3-124">eventHubName</span><span class="sxs-lookup"><span data-stu-id="73ea3-124">eventHubName</span></span>
<span data-ttu-id="73ea3-125">Název centra událostí vytvořeného v oboru názvů Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="73ea3-125">The name of the event hub created in the Event Hubs namespace.</span></span>

```json
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a><span data-ttu-id="73ea3-126">eventHubConsumerGroupName</span><span class="sxs-lookup"><span data-stu-id="73ea3-126">eventHubConsumerGroupName</span></span>
<span data-ttu-id="73ea3-127">Název skupiny příjemců pro centra událostí vytvořit.</span><span class="sxs-lookup"><span data-stu-id="73ea3-127">The name of the consumer group created for the event hub.</span></span>

```json
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a><span data-ttu-id="73ea3-128">apiVersion</span><span class="sxs-lookup"><span data-stu-id="73ea3-128">apiVersion</span></span>
<span data-ttu-id="73ea3-129">Verze rozhraní API šablony.</span><span class="sxs-lookup"><span data-stu-id="73ea3-129">The API version of the template.</span></span>

```json
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a><span data-ttu-id="73ea3-130">Prostředky k nasazení</span><span class="sxs-lookup"><span data-stu-id="73ea3-130">Resources to deploy</span></span>
<span data-ttu-id="73ea3-131">Vytvoří obor názvů typu **EventHubs**, s centra událostí a skupiny příjemců.</span><span class="sxs-lookup"><span data-stu-id="73ea3-131">Creates a namespace of type **EventHubs**, with an event hub and a consumer group.</span></span>

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

## <a name="commands-to-run-deployment"></a><span data-ttu-id="73ea3-132">Příkazy pro spuštění nasazení</span><span class="sxs-lookup"><span data-stu-id="73ea3-132">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="73ea3-133">PowerShell</span><span class="sxs-lookup"><span data-stu-id="73ea3-133">PowerShell</span></span>
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a><span data-ttu-id="73ea3-134">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="73ea3-134">Azure CLI</span></span>
```cli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

## <a name="next-steps"></a><span data-ttu-id="73ea3-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="73ea3-135">Next steps</span></span>
<span data-ttu-id="73ea3-136">Další informace o službě Event Hubs najdete na následujících odkazech:</span><span class="sxs-lookup"><span data-stu-id="73ea3-136">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="73ea3-137">Přehled služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="73ea3-137">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="73ea3-138">Vytvoření centra událostí</span><span class="sxs-lookup"><span data-stu-id="73ea3-138">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="73ea3-139">Nejčastější dotazy k Event Hubs</span><span class="sxs-lookup"><span data-stu-id="73ea3-139">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
