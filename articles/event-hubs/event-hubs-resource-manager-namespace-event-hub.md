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
# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Vytvoření oboru názvů Event Hubs s skupina rozbočovače a příjemce událostí pomocí šablony Azure Resource Manager

Tento článek ukazuje, jak toouse šablonu Azure Resource Manager, vytvoří obor názvů typu služby Event Hubs s rozbočovači jedna událost a jedné skupiny uživatelů. Hello článek ukazuje, jak toodefine prostředky, ke kterým jsou nasazené a jak toodefine parametry, které jsou zadané, když se spustí nasazení hello. Můžete tuto šablonu použít pro vlastní nasazení, nebo si ji přizpůsobit toomeet požadavků

Další informace o vytváření šablon najdete v tématu [Tvorba šablon Azure Resource Manageru][Authoring Azure Resource Manager templates].

Hello úplnou šablonu, najdete v části hello [šablony události rozbočovače a příjemce skupiny] [ Event Hub and consumer group template] na Githubu.

> [!NOTE]
> toocheck pro hello nejnovější šablony, navštivte hello [šablon Azure rychlý Start] [ Azure Quickstart Templates] galerie a vyhledejte Event Hubs.
> 
> 

## <a name="what-will-you-deploy"></a>Co budete nasazovat?
Pomocí této šablony nasadíte na obor názvů služby Event Hubs s centra událostí a skupiny příjemců.

[Služba Event Hubs](event-hubs-what-is-event-hubs.md) zpracovává používanou tooprovide událostí a telemetrie příjem příchozích dat tooAzure v masivním měřítku, s nízkou latencí a vysokou spolehlivostí události.

toorun hello nasazení automaticky, klikněte na následující tlačítko hello:

[![Nasazení tooAzure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry
S Azure Resource Manager, můžete definovat parametry pro hodnoty chcete toospecify při nasazení šablony hello. Šablona Hello zahrnuje části s názvem `Parameters` obsahující všechny hodnoty parametrů hello. Měli byste parametr pro ty hodnoty, které se budou lišit podle toowhich hello prostředí, které nasazujete nebo na základě hello projektu, které nasazujete. Parametry nedefinuje pro hello hodnoty, které vždy zůstávají stejné. Každá hodnota parametru v šabloně hello definuje hello prostředky, které jsou nasazeny.

Šablona Hello definuje hello následující parametry:

### <a name="eventhubnamespacename"></a>eventHubNamespaceName
Název Hello toocreate oboru názvů služby Event Hubs hello.

```json
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName
Hello název centra událostí hello vytvořené v oboru názvů služby Event Hubs hello.

```json
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName
Hello název skupiny příjemců hello vytvořené pro centra událostí hello.

```json
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion
Hello rozhraní API verze hello šablony.

```json
"apiVersion": {
"type": "string"
}
```

## <a name="resources-toodeploy"></a>Toodeploy prostředky
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

## <a name="commands-toorun-deployment"></a>Příkazy toorun nasazení
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI
```cli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

## <a name="next-steps"></a>Další kroky
Další informace o službě Event Hubs návštěvou hello následující odkazy:

* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
* [Vytvoření centra událostí](event-hubs-create.md)
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
