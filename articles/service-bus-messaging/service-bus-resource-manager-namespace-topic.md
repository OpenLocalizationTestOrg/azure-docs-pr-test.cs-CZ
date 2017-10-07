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
# <a name="create-a-service-bus-namespace-with-topic-and-subscription-using-an-azure-resource-manager-template"></a>Vytvoření oboru názvů Service Bus s téma a odběr pomocí šablony Azure Resource Manager

Tento článek ukazuje, jak toouse šablonu Azure Resource Manager, vytvoří obor názvů sběrnice a téma a odběr v daném oboru názvů. Se dozvíte, jak toodefine prostředky, ke kterým jsou nasazené a jak toodefine parametry, které jsou zadané, když se spustí nasazení hello. Můžete tuto šablonu použít pro vlastní nasazení, nebo si ji přizpůsobit toomeet požadavků

Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru][Authoring Azure Resource Manager templates].

Hello úplnou šablonu, najdete v části hello [oboru názvů Service Bus s téma a odběr] [ Service Bus namespace with topic and subscription] šablony.

> [!NOTE]
> Hello následující šablony Azure Resource Manager jsou k dispozici ke stažení a nasazení.
> 
> * [Vytvoření oboru názvů Service Bus](service-bus-resource-manager-namespace.md)
> * [Vytvoření oboru názvů Service Bus pomocí fronty](service-bus-resource-manager-namespace-queue.md)
> * [Vytvoření oboru názvů Service Bus pomocí fronty a autorizační pravidla](service-bus-resource-manager-namespace-auth-rule.md)
> * [Vytvoření oboru názvů Service Bus pomocí tématu, předplatné a pravidla](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> toocheck pro hello nejnovější šablony, navštivte hello [šablon Azure rychlý Start] [ Azure Quickstart Templates] galerie a vyhledejte "Service Bus".
> 
> 

## <a name="what-will-you-deploy"></a>Co budete nasazovat?

Pomocí této šablony nasadíte v oboru názvů Service Bus s téma a odběr.

[Témata a odběry Service Bus](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) poskytovat ve formuláři na více komunikace, *publikování a přihlášení k odběru* vzor.

toorun hello nasazení automaticky, klikněte na následující tlačítko hello:

[![Nasazení tooAzure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

S Azure Resource Manager, můžete definovat parametry pro hodnoty chcete toospecify při nasazení šablony hello. Šablona Hello zahrnuje části s názvem `Parameters` obsahující všechny hodnoty parametru hello. Měli byste parametr pro ty hodnoty, které se liší podle hello prostředí, které nasazujete nebo na základě hello projektu, které nasazujete. Parametry nedefinuje pro hodnoty, které vždy zůstanou hello stejné. Každá hodnota parametru se používá v hello šablony toodefine hello prostředky, které jsou nasazeny.

Šablona Hello definuje hello následující parametry.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
Název Hello toocreate oboru názvů Service Bus hello.

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName
Hello název tématu hello vytvořené v oboru názvů Service Bus hello.

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName
Hello název odběru hello vytvořené v oboru názvů Service Bus hello.

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion
verze rozhraní API služby Service Bus Hello hello šablony.

```json
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-toodeploy"></a>Toodeploy prostředky
Vytvoří standardní oboru názvů Service Bus typu **zasílání zpráv**, tématu a odběru.

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

## <a name="commands-toorun-deployment"></a>Příkazy toorun nasazení
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="next-steps"></a>Další kroky
Nyní, po vytvoření a nasazení prostředků pomocí Azure Resource Manager, se naučíte, jak toomanage tyto prostředky pomocí těchto článků:

* [Správa služby Service Bus pomocí prostředí PowerShell](service-bus-manage-with-ps.md)
* [Správa prostředků služby Service Bus pomocí hello Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus namespace with topic and subscription]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-and-subscription/
