---
title: "aaaTroubleshoot brány virtuální sítě Azure a připojení – prostředí PowerShell | Microsoft Docs"
description: "Tato stránka vysvětluje, jak řešit toouse hello sledovací proces sítě Azure rutiny prostředí PowerShell"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: f6f0a813-38b6-4a1f-8cfc-1dfdf979f595
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: b7dbe6e44e0fa1ea0481223a84098d7a57c068a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a>Řešení potíží s brány virtuální sítě a připojení pomocí prostředí PowerShell sledovací proces sítě Azure

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Sledovací proces sítě obsahuje řadu funkcí, protože se týká toounderstanding síťovým prostředkům v Azure. Jeden z těchto funkcí je prostředek řešení potíží. Řešení potíží s prostředků je možné volat prostřednictvím portálu hello, prostředí PowerShell, rozhraní příkazového řádku nebo REST API. Při volání, sledovací proces sítě kontroluje stav hello bránu virtuální sítě nebo připojení a vrátí nalezených výsledcích.

## <a name="before-you-begin"></a>Než začnete

Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.

Seznam podporovaných brány typy návštěvě [typy podporované brány](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Přehled

Řešení potíží s prostředků poskytuje hello možnosti řešení potíží, které nastat u brány virtuální sítě a připojení. Když je žádosti tooresource řešení potíží s protokoly Probíhá dotaz a prověřovány. Po dokončení kontroly hello výsledky se vrátí. Požadavky na zdroje, řešení problémů s požadavky jsou dlouho spuštěný, což může trvat několik minut tooreturn výsledku. Hello protokoly z řešení potíží se ukládají v kontejneru v účtu úložiště, který je zadán.

## <a name="retrieve-network-watcher"></a>Načtení sledovací proces sítě

prvním krokem Hello je tooretrieve hello sledovací proces sítě instance. Hello `$networkWatcher` proměnné je předán toohello `Start-AzureRmNetworkWatcherResourceTroubleshooting` rutiny v kroku 4.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="retrieve-a-virtual-network-gateway-connection"></a>Načtení připojení brány virtuální sítě

V tomto příkladu je se řešení potíží s prostředků spustili na připojení. Můžete také předat ji bránu virtuální sítě.

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "2to3" -ResourceGroupName "testrg"
```

## <a name="create-a-storage-account"></a>vytvořit účet úložiště

Řešení potíží s prostředků vrátí data o stavu hello hello prostředku, uloží zároveň protokoly tooa úložiště účet toobe zkontrolovat. V tomto kroku vytvoříme účet úložiště, pokud existuje stávající účet úložiště můžete ji použít.

```powershell
$sa = New-AzureRmStorageAccount -Name "contosoexamplesa" -SKU "Standard_LRS" -ResourceGroupName "testrg" -Location "WestCentralUS"
Set-AzureRmCurrentStorageAccount -ResourceGroupName $sa.ResourceGroupName -Name $sa.StorageAccountName
$sc = New-AzureStorageContainer -Name logs
```

## <a name="run-network-watcher-resource-troubleshooting"></a>Spustit Poradce při potížích sledovací proces sítě prostředků

Řešení potíží s prostředky s hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` rutiny. Jsme předat objekt hello rutiny hello sledovací proces sítě, hello Id hello připojení nebo brány virtuální sítě, id účtu úložiště hello a hello cesta toostore hello výsledky.

> [!NOTE]
> Hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` rutina dlouho spuštěný a může trvat několik minut toocomplete.

```powershell
Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath "$($sa.PrimaryEndpoints.Blob)$($sc.name)"
```

Po spuštění rutiny hello sledovací proces sítě zkontroluje stav hello tooverify hello prostředků. Se vrátí výsledky hello toohello prostředí a ukládá protokoly hello výsledků v zadaný účet úložiště hello.

## <a name="understanding-hello-results"></a>Seznámení s výsledky hello

text Hello akce obsahuje obecné pokyny k jak tooresolve hello problém. Pokud může být akce hello problému, je k dispozici odkaz s další pokyny. V případě hello tam, kde není žádná další pokyny, hello odpovědi poskytuje hello url tooopen případu podpory.  Další informace o vlastnostech hello hello odpovědi a co je součástí najdete v článku [řešení sledovací proces sítě – přehled](network-watcher-troubleshoot-overview.md)

Pokyny ke stahování souborů z účty azure storage, najdete v části příliš[Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Jiný nástroj, který je možné je Storage Explorer. Další informace o Storage Explorer naleznete zde na hello následující odkaz: [Storage Explorer](http://storageexplorer.com/)

## <a name="next-steps"></a>Další kroky

Pokud nastavení bylo změněno tohoto připojení VPN zastavit, přečtěte si téma [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack dolů hello pravidla zabezpečení sítě skupiny a zabezpečení, které může být nejistá.
