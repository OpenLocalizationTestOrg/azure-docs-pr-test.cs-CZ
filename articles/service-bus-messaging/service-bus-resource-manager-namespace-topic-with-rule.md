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
# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a>Vytvoření oboru názvů Service Bus pomocí tématu, předplatné a pravidla pomocí šablony Azure Resource Manager

Tento článek ukazuje, jak toouse šablonu Azure Resource Manager, vytvoří obor názvů sběrnice se tématu, předplatné a pravidlo (filtr). Zjistíte, jak toodefine prostředky, ke kterým jsou nasazené a jak toodefine parametry, které jsou zadané, když se spustí nasazení hello. Můžete tuto šablonu použít pro vlastní nasazení, nebo si ji přizpůsobit toomeet požadavků

Další informace o vytváření šablon najdete v tématu [Tvorba šablon Azure Resource Manageru][Authoring Azure Resource Manager templates].

Další informace o postupů a vzorů na prostředky Azure konvence vytváření názvů najdete v tématu [doporučená zásady vytváření názvů pro prostředky Azure][Recommended naming conventions for Azure resources].

Hello úplnou šablonu, najdete v části hello [oboru názvů Service Bus s tématu, předplatné a pravidlo] [ Service Bus namespace with topic, subscription, and rule] šablony.

> [!NOTE]
> Hello následující šablony Azure Resource Manager jsou k dispozici ke stažení a nasazení.
> 
> * [Vytvoření oboru názvů Service Bus pomocí fronty a autorizační pravidla](service-bus-resource-manager-namespace-auth-rule.md)
> * [Vytvoření oboru názvů Service Bus pomocí fronty](service-bus-resource-manager-namespace-queue.md)
> * [Vytvoření oboru názvů Service Bus](service-bus-resource-manager-namespace.md)
> * [Vytvoření oboru názvů Service Bus s téma a odběr](service-bus-resource-manager-namespace-topic.md)
> 
> toocheck pro hello nejnovější šablony, navštivte hello [šablon Azure rychlý Start] [ Azure Quickstart Templates] galerie a vyhledejte Service Bus.
> 
> 

## <a name="what-will-you-deploy"></a>Co budete nasazovat?

Pomocí této šablony můžete nasadit v oboru názvů Service Bus s tématu, předplatné a pravidlo (filtr).

[Témata a odběry Service Bus](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) poskytovat ve formuláři na více komunikace, *publikování a přihlášení k odběru* vzor. Při používání témat a odběrů, součásti distribuované aplikace nekomunikují navzájem přímo, místo toho si zprávy vyměňují prostřednictvím tématu, která slouží jako prostředník. Téma tooa předplatného se podobá virtuální frontě, která obdrží kopii zprávy, které byly odeslány toohello tématu. Filtr na předplatné vám umožní toospecify příjem zpráv odeslaných tooa tématu by měl být použit v konkrétním odběru tématu.

## <a name="what-are-rules-filters"></a>Co jsou pravidla (filtry)?

V mnoha případech je nutné zpracovat zprávy, které mají určité charakteristické vlastnosti různými způsoby. tooenable, můžete nakonfigurovat odběry toofind zprávy, které mají určité vlastnosti a poté proveďte úpravy toothose vlastnosti. I když Service Bus odběry všech zpráv odeslaných toohello tématu najdete kopírovat můžete pouze podmnožinu těchto fronty zpráv toohello virtuální odběru. To se provádí pomocí filtrů odběrů. toolearn Další informace o pravidlech (filtry), najdete v části [pravidla a akce](service-bus-queues-topics-subscriptions.md#rules-and-actions).

toorun hello nasazení automaticky, klikněte na následující tlačítko hello:

[![Nasazení tooAzure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

S Azure Resource Manager, byste měli definovat parametry pro hodnoty chcete toospecify při nasazení šablony hello. Šablona Hello zahrnuje části s názvem `Parameters` obsahující všechny hodnoty parametrů hello. Měli byste parametr pro ty hodnoty, které se liší podle hello prostředí, které nasazujete nebo na základě hello projektu, které nasazujete. Parametry nedefinuje pro hello hodnoty, které vždy zůstávají stejné. Každá hodnota parametru se používá v hello šablony toodefine hello prostředky, které jsou nasazeny.

Šablona Hello definuje hello následující parametry:

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
### <a name="servicebusrulename"></a>serviceBusRuleName
Název Hello hello rule(filter) vytvořené v oboru názvů Service Bus hello.

```json
   "serviceBusRuleName": {
   "type": "string",
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
Vytvoří standardní oboru názvů Service Bus typu **zasílání zpráv**, tématu a odběru a pravidla.

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

## <a name="commands-toorun-deployment"></a>Příkazy toorun nasazení
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a>Další kroky
Nyní, po vytvoření a nasazení prostředků pomocí Azure Resource Manager, se naučíte, jak toomanage tyto prostředky pomocí těchto článků:

* [Správa služby Azure Service Bus](service-bus-management-libraries.md)
* [Správa služby Service Bus pomocí prostředí PowerShell](service-bus-manage-with-ps.md)
* [Správa prostředků služby Service Bus pomocí hello Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Recommended naming conventions for Azure resources]: ../guidance/guidance-naming-conventions.md
[Service Bus namespace with topic, subscription, and rule]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md

