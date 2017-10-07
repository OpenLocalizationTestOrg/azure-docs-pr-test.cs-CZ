---
title: "prostředky Azure Service Bus aaaCreate pomocí šablony Azure Resource Manager | Microsoft Docs"
description: "Pomocí šablony Azure Resource Manager tooautomate hello vytváření prostředků služby Service Bus"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 24f6a207-0fa4-49cf-8a58-963f9e2fd655
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: e539902cae307b63ae7c332580e2064761331ec5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Vytvoření služby Service Bus prostředků pomocí šablony Azure Resource Manager

Tento článek popisuje, jak toocreate a nasazení prostředků služby Service Bus pomocí šablony Azure Resource Manager, prostředí PowerShell a poskytovatele prostředků hello Service Bus.

Šablony Azure Resource Manageru můžete definovat hello toodeploy prostředky pro řešení a toospecify parametry a proměnné, které umožňují tooinput hodnoty pro různá prostředí. Šablona Hello se skládá z JSON a výrazy, můžete použít hodnoty tooconstruct pro vaše nasazení. Podrobné informace o vytváření šablon Azure Resource Manageru a diskuzi o formátu hello šablony najdete v tématu [syntaxi šablon Azure Resource Manager a struktura](../azure-resource-manager/resource-group-authoring-templates.md).

> [!NOTE]
> jak Hello příklady v této zobrazit článek toouse Azure Resource Manager toocreate oboru názvů Service Bus a zasílání zpráv entity (queue). Další příklady šablony najdete v článku hello [galerii šablon Azure rychlý Start] [ Azure Quickstart Templates gallery] a vyhledejte "Service Bus".
>
>

## <a name="service-bus-resource-manager-templates"></a>Šablony služby sběrnice Resource Manager

Tyto šablony správce prostředků Azure Service Bus jsou k dispozici ke stažení a nasazení. Klikněte na tlačítko hello následující odkazy podrobnosti o každém z nich, se šablonami toohello odkazy na Githubu:

* [Vytvoření oboru názvů Service Bus](service-bus-resource-manager-namespace.md)
* [Vytvoření oboru názvů Service Bus pomocí fronty](service-bus-resource-manager-namespace-queue.md)
* [Vytvoření oboru názvů Service Bus s téma a odběr](service-bus-resource-manager-namespace-topic.md)
* [Vytvoření oboru názvů Service Bus pomocí fronty a autorizační pravidla](service-bus-resource-manager-namespace-auth-rule.md)
* [Vytvoření oboru názvů Service Bus pomocí tématu, předplatné a pravidla](service-bus-resource-manager-namespace-topic-with-rule.md)

## <a name="deploy-with-powershell"></a>Nasazení s využitím PowerShellu

Hello následující postup popisuje, jak toouse prostředí PowerShell toodeploy šablonu Azure Resource Manager vytvářející **standardní** vrstvy oboru názvů Service Bus a fronty v daném oboru názvů. Tento příklad vychází z hello [vytvoření oboru názvů Service Bus s frontou](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) šablony. Hello přibližnou pracovní postup je následující:

1. Instalace prostředí PowerShell.
2. Vytvořte šablonu hello a (volitelně) ze souboru parametrů.
3. V prostředí PowerShell Přihlaste se tooyour účet Azure.
4. Pokud žádný neexistuje, vytvořte novou skupinu prostředků.
5. Test nasazení hello.
6. V případě potřeby nastavte režim nasazení hello.
7. Nasazení šablony hello.

Úplné informace o nasazení šablony Azure Resource Manager najdete v tématu [nasazení prostředků pomocí šablony Azure Resource Manager][Deploy resources with Azure Resource Manager templates].

### <a name="install-powershell"></a>Instalace PowerShellu

Nainstalovat Azure PowerShell podle následujících pokynů hello v [Začínáme s Azure Powershellem](/powershell/azure/get-started-azureps).

### <a name="create-a-template"></a>Vytvoření šablony

Klonování nebo kopírování hello [201-servicebus vytvořit queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) šablony z Githubu:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of hello Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of hello Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by hello template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
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
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a>Vytvořte soubor parametrů (volitelné)

toouse soubor volitelné parametry kopie hello [201-servicebus vytvořit queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) souboru. Nahraďte hodnotu hello `serviceBusNamespaceName` hello název oboru názvů Service Bus hello chcete toocreate v tomto nasazení a nahraďte hodnotu hello `serviceBusQueueName` s názvem hello hello fronty chcete toocreate.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

Další informace najdete v tématu hello [parametry](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) tématu.

### <a name="log-in-tooazure-and-set-hello-azure-subscription"></a>Přihlaste se tooAzure a nastavte hello předplatného Azure

Příkazovém řádku prostředí PowerShell spusťte následující příkaz hello:

```powershell
Login-AzureRmAccount
```

Jste výzvami toolog na tooyour účet Azure. Po přihlášení, spusťte následující příkaz tooview hello dostupných předplatných.

```powershell
Get-AzureRMSubscription
```

Tento příkaz vrátí seznam dostupných předplatných Azure. Zvolte předplatné pro hello aktuální relace tak, že spustíte následující příkaz hello. Nahraďte `<YourSubscriptionId>` s hello identifikátor GUID pro hello předplatné chcete toouse.

```powershell
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-hello-resource-group"></a>Skupina prostředků hello sady

Pokud nemáte existující prostředek skupiny, vytvořte novou skupinu prostředků s hello ** New-AzureRmResourceGroup ** příkaz. Zadejte název hello hello skupinu prostředků a umístění, které chcete toouse. Například:

```powershell
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Pokud bylo úspěšné, zobrazí se souhrn hello novou skupinu prostředků.

```powershell
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-hello-deployment"></a>Testovací nasazení pro hello

Ověření nasazení spuštěním hello `Test-AzureRmResourceGroupDeployment` rutiny. Při testování hello nasazení, zadejte přesně tak, jak by při provádění nasazení hello parametry.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

### <a name="create-hello-deployment"></a>Vytvoření nasazení hello

nové nasazení hello toocreate, spusťte hello `New-AzureRmResourceGroupDeployment` rutiny a zadejte potřebné parametry hello po zobrazení výzvy. Hello parametry jsou název pro vaše nasazení hello název vaší skupiny prostředků a hello cesta nebo adresa URL souboru šablony toohello. Pokud hello **režimu** není zadán parametr, hello výchozí hodnotu **přírůstkové** se používá. Další informace najdete v tématu [přírůstkové a úplné nasazení](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments).

Hello následující příkazové řádky můžete pro hello tři parametry v okně PowerShell hello:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

toospecify soubor parametrů místo toho použijte následující příkaz hello.

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json -TemplateParameterFile <path tooparameters file>\azuredeploy.parameters.json
```

Vložené parametry můžete použít také při spuštění rutiny nasazení hello. příkaz Hello vypadá takto:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json -parameterName "parameterValue"
```

toorun [dokončení](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) nasazení, sada hello **režimu** parametr příliš**Complete**:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

### <a name="verify-hello-deployment"></a>Ověření nasazení hello
Pokud hello prostředky nasadí úspěšně, zobrazí se v okně PowerShell hello shrnutí nasazení hello:

```powershell
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a>Další kroky
Nyní jste se seznámili hello základní pracovní postup a příkazy pro nasazení šablonu Azure Resource Manager. Podrobnější informace najdete v článku hello následující odkazy:

* [Přehled Azure Resource Manageru][Azure Resource Manager overview]
* [Nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell][Deploy resources with Azure Resource Manager templates]
* [Tvorba šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md)

[Azure Resource Manager overview]: ../azure-resource-manager/resource-group-overview.md
[Deploy resources with Azure Resource Manager templates]: ../azure-resource-manager/resource-group-template-deploy.md
[Azure Quickstart Templates gallery]: https://azure.microsoft.com/documentation/templates/?term=service+bus
