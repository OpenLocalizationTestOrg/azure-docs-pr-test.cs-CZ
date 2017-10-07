---
title: "prostředky Azure Service Bus toomanage prostředí PowerShell aaaUse | Microsoft Docs"
description: "Použít toocreate modulu prostředí PowerShell a správu prostředků služby Service Bus"
services: service-bus-messaging
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/06/2017
ms.author: sethm
ms.openlocfilehash: 737044def913c5798e7e05fc4f1aeece76c8f4dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toomanage-service-bus-resources"></a>Použití prostředků služby Service Bus toomanage prostředí PowerShell

Microsoft Azure PowerShell je skriptovací prostředí, můžete použít toocontrol a automatizovat hello nasazení a správu služeb Azure. Tento článek popisuje, jak toouse hello [modulu Powershellu pro Service Bus Resource Manager](/powershell/module/azurerm.servicebus) tooprovision a spravovat entit služby Service Bus (obory názvů, fronty, témata a odběry) pomocí místní konzoly Azure PowerShell nebo skript.

Můžete také spravovat entit služby Service Bus pomocí šablony Azure Resource Manager. Další informace najdete v článku hello [vytvořit Service Bus prostředků pomocí šablony Azure Resource Manager](service-bus-resource-manager-overview.md).

## <a name="prerequisites"></a>Požadavky

Než začnete, budete potřebovat následující hello:

* Předplatné Azure. Další informace o získání předplatného najdete v tématu [možností nákupu][purchase options], [nabízí člen][member offers], nebo [bezplatný účet][free account].
* Počítač s prostředím Azure PowerShell. Pokyny najdete v tématu [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/get-started-azureps).
* Obecné znalosti skriptů prostředí PowerShell, balíčků NuGet a hello rozhraní .NET Framework.

## <a name="get-started"></a>Začínáme

prvním krokem Hello je toouse toolog prostředí PowerShell v tooyour účet Azure a předplatné Azure. Postupujte podle pokynů hello v [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/get-started-azureps) toolog tooyour účet Azure a načtení a přístup hello prostředky ve vašem předplatném Azure.

## <a name="provision-a-service-bus-namespace"></a>Zřízení oboru názvů Service Bus

Při práci s obory názvů Service Bus, můžete použít hello [Get-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace), [New-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace), [Remove-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace), a [Set-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespace) rutiny.

Tento příklad vytvoří pár lokální proměnné ve skriptu hello; `$Namespace` a `$Location`.

* `$Namespace`je název oboru názvů Service Bus hello se kterým má být toowork hello.
* `$Location`identifikuje hello datového centra, ve kterém jsme zřizovat hello oboru názvů.
* `$CurrentNamespace`úložiště hello odkaz na obor názvů, který jsme načíst (nebo ji vytvořte).

Ve skriptu skutečné `$Namespace` a `$Location` lze předat jako parametry.

Tato část skriptu hello hello následující:

1. Pokusy o tooretrieve v oboru názvů Service Bus s hello zadán název.
2. Pokud je nalezen hello obor názvů, sestavy, co byl nalezen.
3. Pokud není nalezen hello obor názvů, vytvoří obor názvů hello a pak načte hello nově vytvořený obor názvů.
   
    ``` powershell
    # Query toosee if hello namespace currently exists
    $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
   
    # Check if hello namespace already exists or needs toobe created
    if ($CurrentNamespace)
    {
        Write-Host "hello namespace $Namespace already exists in hello $Location region:"
        # Report what was found
        Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "hello $Namespace namespace does not exist."
        Write-Host "Creating hello $Namespace namespace in hello $Location region..."
        New-AzureRmServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
        Write-Host "hello $Namespace namespace in Resource Group $ResGrpName in hello $Location region has been successfully created."
                
    }
    ```

### <a name="create-a-namespace-authorization-rule"></a>Vytvoření oboru názvů autorizační pravidla

Hello následující příklad ukazuje, jak toomanage obor názvů autorizační pravidla pomocí hello [New-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespaceauthorizationrule), [Get-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespaceauthorizationrule), [Set-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespaceauthorizationrule), a [rutiny Remove-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespaceauthorizationrule).

```powershell
# Query toosee if rule exists
$CurrentRule = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

# Check if hello rule already exists or needs toobe created
if ($CurrentRule)
{
    Write-Host "hello $AuthRule rule already exists for hello namespace $Namespace."
}
else
{
    Write-Host "hello $AuthRule rule does not exist."
    Write-Host "Creating hello $AuthRule rule for hello $Namespace namespace..."
    New-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule -Rights @("Listen","Send")
    $CurrentRule = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
    Write-Host "hello $AuthRule rule for hello $Namespace namespace has been successfully created."

    Write-Host "Setting rights on hello namespace"
    $authRuleObj = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

    Write-Host "Remove Send rights"
    $authRuleObj.Rights.Remove("Send")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Add Send and Manage rights toohello namespace"
    $authRuleObj.Rights.Add("Send")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj
    $authRuleObj.Rights.Add("Manage")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Show value of primary key"
    $CurrentKey = Get-AzureRmServiceBusNamespaceKey -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
        
    Write-Host "Remove this authorization rule"
    Remove-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
}
```

## <a name="create-a-queue"></a>Vytvoření fronty

toocreate fronta nebo téma, proveďte kontrolu oboru názvů pomocí skriptu hello v předchozí části hello. Pak vytvořte hello fronty:

```powershell
# Check if queue already exists
$CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName

if($CurrentQ)
{
    Write-Host "hello queue $QueueName already exists in hello $Location region:"
}
else
{
    Write-Host "hello $QueueName queue does not exist."
    Write-Host "Creating hello $QueueName queue in hello $Location region..."
    New-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -EnablePartitioning $True
    $CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName
    Write-Host "hello $QueueName queue in Resource Group $ResGrpName in hello $Location region has been successfully created."
}
```

### <a name="modify-queue-properties"></a>Úprava vlastností fronty.

Po provedení hello skript v předcházející části hello, můžete použít hello [Set-AzureRmServiceBusQueue](/powershell/module/azurerm.servicebus/set-azurermservicebusqueue) rutiny tooupdate hello vlastností fronty, stejně jako hello následující ukázka:

```powershell
$CurrentQ.DeadLetteringOnMessageExpiration = $True
$CurrentQ.MaxDeliveryCount = 7
$CurrentQ.MaxSizeInMegabytes = 2048
$CurrentQ.EnableExpress = $True

Set-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -QueueObj $CurrentQ
```

## <a name="provisioning-other-service-bus-entities"></a>Zřizování dalšími subjekty, Service Bus

Můžete použít hello [modulu Service Bus PowerShell](/powershell/module/azurerm.servicebus) tooprovision jinými entitami, jako jsou témata a odběry. Tyto rutiny jsou syntakticky podobné toohello fronty vytvoření rutiny ukázáno v předchozí části hello.

## <a name="next-steps"></a>Další kroky

- V tématu hello kompletní dokumentaci modulu Powershellu pro Service Bus Resource Manager [zde](/powershell/module/azurerm.servicebus). Tato stránka obsahuje seznam všech dostupných rutin.
- Informace o používání šablon Azure Resource Manageru, najdete v článku hello [vytvořit Service Bus prostředků pomocí šablony Azure Resource Manager](service-bus-resource-manager-overview.md).
- Informace o [knihovny Service Bus .NET správy](service-bus-management-libraries.md).

Existují některé alternativní způsoby toomanage entit služby Service Bus, jak je popsáno v těchto příspěvcích na blogu:

* [Jak toocreate Service Bus fronty, témata a odběry pomocí skriptu prostředí PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Jak toocreate a Service Bus Namespace a Centrum událostí pomocí skriptu prostředí PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)
* [Skripty prostředí PowerShell služby Service Bus](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
