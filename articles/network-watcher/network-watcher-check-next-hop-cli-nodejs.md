---
title: "Další směrování aaaFind s Azure sítě sledovacích procesů další segment - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje, jak zjistíte, jaké hello dalšího směrování typ je a pomocí ip adresa dalšího směrování pomocí rozhraní příkazového řádku Azure."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 0700c274-3e0d-4dca-acfa-3ceac8990613
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 54124c051021413695d70ba93c370605abc6ebbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-azure-cli-10"></a>Zjistěte, jaký typ hello dalšího směrování je díky funkci další segment hello v sledovací proces sítě Azure pomocí Azure CLI 1.0

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Rozhraní API Azure REST](network-watcher-check-next-hop-rest.md)

Další směrování je funkce sledovací proces sítě, která poskytuje možnost hello získat hello typ dalšího směrování a IP adresu podle zadaný virtuální počítač. Tato funkce je užitečná při určování, zda je brána, internet nebo cílové tooits tooget virtuální sítě prochází přes odchozího provozu z virtuálního počítače.

Tento článek používá 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux.

## <a name="before-you-begin"></a>Než začnete

V tomto scénáři použijete hello rozhraní příkazového řádku Azure toofind hello typ dalšího směrování a IP adresu.

Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě. scénář Hello také předpokládá, že skupina prostředků se platný virtuální počítač existuje toobe použít.

## <a name="scenario"></a>Scénář

scénář Hello popsaná v tomto článku používá další směrování, funkce sledovací proces sítě, který vyhledá hello typ dalšího směrování a IP adresu pro prostředek. toolearn Další informace o další směrování, navštivte [další směrování přehled](network-watcher-next-hop-overview.md).


## <a name="get-next-hop"></a>Získat další směrování

tooget hello další segment říkáme hello `azure netowrk watcher next-hop` rutiny. Jsme předat skupinu prostředků hello rutiny hello sledovací proces sítě, hello NetworkWatcher, virtuálního počítače Id, zdrojové IP adresy a cílové IP adresy. V tomto příkladu je hello cílové IP adresy tooa virtuálních počítačů v jiné virtuální síti. Mezi hello dvě virtuální sítě je bránu virtuální sítě. 

```azurecli
azure network watcher next-hop -g resourceGroupName -n networkWatcherName -t targetResourceId -a <source-ip> -d <destination-ip>
```

> [!NOTE]
Pokud hello virtuální počítač má několik síťových adaptérů a předávání IP je povolena na žádném z hello síťové adaptéry, pak hello parametr síťový adaptér (-i síťový adaptér id) musí být zadán. V opačném případě je volitelný.

## <a name="review-results"></a>Zkontrolujte výsledky

Po dokončení, jsou uvedené výsledky hello. IP adresa dalšího směrování Hello je i hello typ prostředku, který je vrácen.

```
data:    Next Hop Ip Address             : 10.0.1.2
info:    network watcher next-hop command OK
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
