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
ms.date: 10/09/2017
ms.author: sethm;shvija
ms.openlocfilehash: 4cc9a0b9eaabb15a5a316e094deb178ef2219692
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Vytvoření oboru názvů Event Hubs s skupina rozbočovače a příjemce událostí pomocí šablony Azure Resource Manager

Tento článek ukazuje, jak používat šablonu Azure Resource Manager, která vytvoří obor názvů typu Event Hubs s rozbočovači jedna událost a jedna skupina příjemců. Článek popisuje, jak definovat jsou nasazené prostředky, ke kterým a jak definovat parametry, které jsou zadané, když se spustí nasazení. Tuto šablonu můžete použít pro vlastní nasazení nebo ji upravit, aby splňovala vaše požadavky.

Další informace o vytváření šablon najdete v tématu [Tvorba šablon Azure Resource Manageru][Authoring Azure Resource Manager templates].

Úplnou šablonu, najdete v článku [šablony události rozbočovače a příjemce skupiny] [ Event Hub and consumer group template] na Githubu.

> [!NOTE]
> Nejnovější šablony můžete zkontrolovat tak, že přejdete do galerie [Šablony Azure pro rychlý start][Azure Quickstart Templates] a vyhledáte Event Hubs.
> 
> 

## <a name="what-will-you-deploy"></a>Co budete nasazovat?
Pomocí této šablony nasadíte na obor názvů služby Event Hubs s centra událostí a skupiny příjemců.

Služba [Event Hubs](event-hubs-what-is-event-hubs.md) zpracovává události a zajišťuje příjem příchozích dat událostí a telemetrie do Azure v masivním měřítku, s nízkou latencí a vysokou spolehlivostí.

Pokud chcete nasazení spustit automaticky, klikněte na následující tlačítko:

[![Nasazení do Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry
Pomocí Azure Resource Manageru definujete parametry pro hodnoty, které chcete zadat při nasazení šablony. Šablona obsahuje část `Parameters`, která obsahuje všechny hodnoty parametrů. Měli byste parametr pro ty hodnoty, které se budou lišit podle prostředí, do které nasazujete nebo na základě projektu, které nasazujete. Nedefinujte parametry pro hodnoty, které jsou vždy stejné. Každá hodnota parametru v šabloně definuje prostředky, které jsou nasazeny.

Šablona definuje následující parametry:

### <a name="eventhubnamespacename"></a>eventHubNamespaceName
Název oboru názvů Event Hubs, který se má vytvořit.

```json
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName
Název centra událostí vytvořeného v oboru názvů Event Hubs.

```json
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName
Název skupiny příjemců pro centra událostí vytvořit.

```json
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion
Verze rozhraní API šablony.

```json
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Prostředky k nasazení
Vytvoří obor názvů typu **EventHubs**, s centra událostí a skupiny příjemců.

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

## <a name="commands-to-run-deployment"></a>Příkazy pro spuštění nasazení
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

## <a name="next-steps"></a>Další kroky
Další informace o službě Event Hubs najdete na následujících odkazech:

* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
* [Vytvoření centra událostí](event-hubs-create.md)
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
