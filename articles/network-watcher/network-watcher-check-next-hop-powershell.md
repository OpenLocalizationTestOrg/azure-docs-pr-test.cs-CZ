---
title: "Další směrování aaaFind s Azure sítě sledovacích procesů další segment – prostředí PowerShell | Microsoft Docs"
description: "Tento článek popisuje, jak zjistíte, jaké hello dalšího směrování typ je a pomocí ip adresa dalšího směrování pomocí prostředí PowerShell."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 6a656c55-17bd-40f1-905d-90659087639c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: fdb0b4a02d95fc45c103fe952fc1afa095414c18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-powershell"></a>Zjistěte, jaký typ hello dalšího směrování je díky funkci další segment hello v sledovací proces sítě Azure pomocí prostředí PowerShell

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Rozhraní API Azure REST](network-watcher-check-next-hop-rest.md)

Další směrování je funkce sledovací proces sítě, která poskytuje možnost hello získat hello typ dalšího směrování a IP adresu podle zadaný virtuální počítač. Tato funkce je užitečná při určování, zda je brána, internet nebo cílové tooits tooget virtuální sítě prochází přes odchozího provozu z virtuálního počítače.

## <a name="before-you-begin"></a>Než začnete

V tomto scénáři použijete hello Azure portálu toofind hello typ dalšího směrování a IP adresu.

Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě. scénář Hello také předpokládá, že skupina prostředků se platný virtuální počítač existuje toobe použít.

## <a name="scenario"></a>Scénář

scénář Hello popsaná v tomto článku používá další směrování, funkce sledovací proces sítě, který vyhledá hello typ dalšího směrování a IP adresu pro prostředek. toolearn Další informace o další směrování, navštivte [další směrování přehled](network-watcher-next-hop-overview.md).

## <a name="retrieve-network-watcher"></a>Načtení sledovací proces sítě

prvním krokem Hello je tooretrieve hello sledovací proces sítě instance. Hello `$networkWatcher` proměnné je předán další segment toohello ověřte rutiny.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-virtual-machine"></a>Získat virtuální počítač

Další směrování vrátí další segment hello a hello IP adresa dalšího směrování hello z virtuálního počítače. Id virtuálního počítače je vyžadováno pro rutinu hello. Pokud už znáte ID hello toouse hello virtuálního počítače, můžete tento krok přeskočit.

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

> [!NOTE]
> Další směrování vyžaduje, aby hello prostředků virtuálního počítače je přidělená toorun.

## <a name="get-hello-network-interfaces"></a>Získat hello síťová rozhraní

v tomto příkladu, nemůžeme načíst hello síťové adaptéry na virtuálním počítači, je potřeba Hello IP adresa síťového adaptéru na virtuálním počítači hello. Pokud už znáte hello IP adresu, kterou chcete tootest hello virtuálního počítače, můžete tento krok přeskočit.

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="get-next-hop"></a>Získat další směrování

Teď říkáme hello `Get-AzureRmNetworkWatcherNextHop` rutiny. Jsme předat hello rutiny hello sledovací proces sítě, virtuální počítač Id, zdrojovou IP adresu a cílové IP adresy. V tomto příkladu je hello cílové IP adresy tooa virtuálních počítačů v jiné virtuální síti. Mezi hello dvě virtuální sítě je bránu virtuální sítě.

```powershell
Get-AzureRmNetworkWatcherNextHop -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id -SourceIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress  -DestinationIPAddress 10.0.2.4 
```

## <a name="review-results"></a>Zkontrolujte výsledky

Po dokončení, jsou uvedené výsledky hello. IP adresa dalšího směrování Hello je i hello typ prostředku, který je vrácen. V tomto scénáři je hello veřejnou IP adresu brány virtuální sítě hello.

```
NextHopIpAddress NextHopType           RouteTableId 
---------------- -----------           ------------ 
13.78.238.92     VirtualNetworkGateway Gateway Route
```

Hello následující seznam obsahuje hello aktuálně k dispozici NextHopType hodnoty:

**Typ dalšího směrování**

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* Žádný

## <a name="next-steps"></a>Další kroky

Zjistěte, jak tooreview nastavení skupiny zabezpečení sítě prostřednictvím kódu programu přechodem [NSG auditování s sledovací proces sítě](network-watcher-nsg-auditing-powershell.md)

















