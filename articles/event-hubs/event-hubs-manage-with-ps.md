---
title: "prostředky Azure Event Hubs toomanage prostředí PowerShell aaaUse | Microsoft Docs"
description: "Použít toocreate modulu prostředí PowerShell a spravovat služby Event Hubs"
services: event-hubs
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: d79cb307c2b4a031d059ce6ca67117ffc0b4600b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toomanage-event-hubs-resources"></a>Pomocí prostředí PowerShell toomanage Event Hubs prostředky

Microsoft Azure PowerShell je skriptovací prostředí, můžete použít toocontrol a automatizovat hello nasazení a správu služeb Azure. Tento článek popisuje, jak toouse hello [modulu PowerShell Resource Manager centra událostí](/powershell/module/azurerm.eventhub) tooprovision a spravovat služby Event Hubs entity (obory názvů, jednotlivé služba event hubs a skupiny uživatelů) pomocí místní konzoly Azure PowerShell nebo skript.

Můžete také spravovat služby Event Hubs prostředků pomocí šablony Azure Resource Manager. Další informace najdete v článku hello [vytvoření oboru názvů Event Hubs s skupina rozbočovače a příjemce událostí pomocí šablony Azure Resource Manager](event-hubs-resource-manager-namespace-event-hub.md).

## <a name="prerequisites"></a>Požadavky

Než začnete, budete potřebovat následující hello:

* Předplatné Azure. Další informace o získání předplatného najdete v tématu [možností nákupu][purchase options], [nabízí člen][member offers], nebo [bezplatný účet][free account].
* Počítač s prostředím Azure PowerShell. Pokyny najdete v tématu [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/get-started-azureps).
* Obecné znalosti skriptů prostředí PowerShell, balíčků NuGet a hello rozhraní .NET Framework.

## <a name="get-started"></a>Začínáme

prvním krokem Hello je toouse toolog prostředí PowerShell v tooyour účet Azure a předplatné Azure. Postupujte podle pokynů hello v [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/get-started-azureps) toolog v tooyour účet Azure, pak načíst a přístup k prostředkům hello ve vašem předplatném Azure.

## <a name="provision-an-event-hubs-namespace"></a>Zřídit na obor názvů služby Event Hubs

Při práci se službou Event Hubs obory názvů, můžete použít hello [Get-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/get-azurermeventhubnamespace), [New-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/new-azurermeventhubnamespace), [odebrat AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/remove-azurermeventhubnamespace) , a [Set-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/set-azurermeventhubnamespace) rutiny.

Tento příklad vytvoří pár lokální proměnné ve skriptu hello; `$Namespace` a `$Location`.

* `$Namespace`je název hello hello Event Hubs oboru názvů, ke kterému má být toowork.
* `$Location`identifikuje hello datového centra, ve kterém jsme zřizovat hello oboru názvů.
* `$CurrentNamespace`úložiště hello odkaz na obor názvů, který jsme načíst (nebo ji vytvořte).

Ve skriptu skutečné `$Namespace` a `$Location` lze předat jako parametry.

Tato část skriptu hello hello následující:

1. Pokusy o tooretrieve na obor názvů služby Event Hubs s hello zadán název.
2. Pokud je nalezen hello obor názvů, sestavy, co byl nalezen.
3. Pokud není nalezen hello obor názvů, vytvoří obor názvů hello a pak načte hello nově vytvořený obor názvů.

    ```powershell
    # Query toosee if hello namespace currently exists
    $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
   
    # Check if hello namespace already exists or needs toobe created
    if ($CurrentNamespace)
    {
        Write-Host "hello namespace $Namespace already exists in hello $Location region:"
        # Report what was found
        Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "hello $Namespace namespace does not exist."
        Write-Host "Creating hello $Namespace namespace in hello $Location region..."
        New-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
        Write-Host "hello $Namespace namespace in Resource Group $ResGrpName in hello $Location region has been successfully created."
    }
    ```

## <a name="create-an-event-hub"></a>Vytvoření centra událostí

toocreate centra událostí, proveďte kontrolu oboru názvů pomocí skriptu hello v předchozí části hello. Poté použijte hello [New-AzureRmEventHub](/powershell/module/azurerm.eventhub/new-azurermeventhub) centra událostí hello toocreate rutiny:

```powershell
# Check if event hub already exists
$CurrentEH = Get-AzureRMEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName

if($CurrentEH)
{
    Write-Host "hello event hub $EventHubName already exists in hello $Location region:"
    # Report what was found
    Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "hello $EventHubName event hub does not exist."
    Write-Host "Creating hello $EventHubName event hub in hello $Location region..."
    New-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -Location $Location -MessageRetentionInDays 3
    $CurrentEH = Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "hello $EventHubName event hub in Resource Group $ResGrpName in hello $Location region has been successfully created."
}
```

### <a name="create-a-consumer-group"></a>Vytvořte skupinu uživatelů

toocreate skupiny příjemců v Centru událostí provést hello oboru názvů událostí rozbočovače kontroly a pomocí skriptů hello v předchozí části hello. Poté použijte hello [New-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/new-azurermeventhubconsumergroup) rutiny toocreate hello skupiny uživatelů v rámci centra událostí hello. Například:

```powershell
# Check if consumer group already exists
$CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -ErrorAction Ignore

if($CurrentCG)
{
    Write-Host "hello consumer group $ConsumerGroupName in event hub $EventHubName already exists in hello $Location region:"
    # Report what was found
    Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "hello $ConsumerGroupName consumer group does not exist."
    Write-Host "Creating hello $ConsumerGroupName consumer group in hello $Location region..."
    New-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
    $CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "hello $ConsumerGroupName consumer group in event hub $EventHubName in Resource Group $ResGrpName in hello $Location region has been successfully created."
}
```

#### <a name="set-user-metadata"></a>Metadata uživatele sady

Po provedení hello skripty v předchozích částech hello, můžete použít hello [Set-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/set-azurermeventhubconsumergroup) rutiny tooupdate hello vlastnosti skupiny příjemců, stejně jako hello následující ukázka:

```powershell
# Set some user metadata on hello CG
Write-Host "Setting hello UserMetadata field too'Testing'"
Set-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -UserMetadata "Testing"
# Show result
Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
```

## <a name="remove-event-hub"></a>Odebrat centra událostí

Služba event hubs hello tooremove jste vytvořili, můžete použít hello `Remove-*` rutiny jako hello následující ukázka:

```powershell
# Clean up
Remove-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
Remove-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
Remove-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
```

## <a name="next-steps"></a>Další kroky

- Najdete v dokumentaci hello dokončení Powershellu Resource Manager centra událostí modulu [zde](/powershell/module/azurerm.eventhub). Tato stránka obsahuje seznam všech dostupných rutin.
- Informace o používání šablon Azure Resource Manageru, najdete v článku hello [vytvoření oboru názvů Event Hubs s skupina rozbočovače a příjemce událostí pomocí šablony Azure Resource Manager](event-hubs-resource-manager-namespace-event-hub.md).
- Informace o [knihovny .NET centra událostí správy](event-hubs-management-libraries.md).

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
