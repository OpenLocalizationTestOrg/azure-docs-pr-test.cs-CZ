---
title: "aaaCreate instance sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje kroky toocreate hello instanci sledovací proces sítě pomocí portálu pro hello a REST API služby Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: b1314119-0b87-4f4d-b44c-2c4d0547fb76
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 90d4f90c9709a80e4b27863e79e5b6e16de145c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-network-watcher-instance"></a>Vytvoření instance sledovací proces sítě Azure

Sledovací proces sítě je místní služba, která umožňuje vám toomonitor a diagnostikovat podmínky na úrovni scénář sítě, do a z Azure. Scénář úrovně monitorování umožňuje toodiagnose problémy v síti tooend koncové pro úroveň zobrazení. Diagnostika sítě a k dispozici sledovací proces sítě vizualizace nástroje vám pomůžou pochopit, diagnostikovat a získat statistiky tooyour sítě v Azure.

> [!NOTE]
> Sledovací proces sítě aktuálně podporuje pouze rozhraní příkazového řádku 1.0, hello pokyny toocreate novou instanci sledovací proces sítě se poskytuje pro rozhraní příkazového řádku 1.0.

## <a name="create-a-network-watcher-in-hello-portal"></a>Vytvoření sledovací proces sítě hello portálu

Přejděte příliš**více služeb** > **sítě** > **sledovací proces sítě**. Můžete vybrat všechny odběry hello chcete tooenable sledovací proces sítě pro. Tato akce vytvoří sledovací proces sítě v každé oblasti, která je k dispozici.

![Vytvoření sledovací proces sítě][1]

Když povolíte sledovací proces sítě pomocí hello portál, název hello instance hello sledovací proces sítě se automaticky nastaví tooNetworkWatcher_region_name kde region_name odpovídá toohello oblast Azure, podporou hello instance.  Například sledovací proces sítě povolené v oblasti západní centrální USA bude mít název NetworkWatcher_westcentralus

Kromě toho instance hello sledovací proces sítě se automaticky přidá do skupinu prostředků s názvem NetworkWatcherRG.  Tato skupina prostředků bude vytvořen, pokud ještě neexistuje.

Pokud chcete toocustomize hello název instance sledovací proces sítě a hello skupina prostředků je umístěn do, můžete použít prostředí Powershell, hello REST API nebo ARMClient metod popsaných níže.  V každé možnosti musí existovat hello skupiny prostředků než umístíte hello sledovací proces sítě do ní.  

## <a name="create-a-network-watcher-with-powershell"></a>Vytvoření sledovací proces sítě pomocí prostředí PowerShell

toocreate instanci sledovací proces sítě, spusťte hello následující ukázka:

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-hello-rest-api"></a>Vytvoření sledovací proces sítě s hello REST API

ARMclient je použité toocall hello REST API pomocí prostředí PowerShell. ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)

### <a name="log-in-with-armclient"></a>Přihlaste se pomocí ARMClient

```powerShell
armclient login
```

### <a name="create-hello-network-watcher"></a>Vytvoření sledovací proces sítě hello

```powershell
$subscriptionId = '<subscription id>'
$networkWatcherName = '<name of network watcher>'
$resourceGroupName = '<resource group name>'
$apiversion = "2016-09-01"
$requestBody = @"
{
'location': 'West Central US'
}
"@

armclient put "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}?api-version=${api-version}" $requestBody
```

## <a name="next-steps"></a>Další kroky

Teď, když máte instanci sledovací proces sítě, další informace o hello funkce, které jsou k dispozici:

* [Topologie](network-watcher-topology-overview.md)
* [Zachytávání paketů](network-watcher-packet-capture-overview.md)
* [Ověření IP toku](network-watcher-ip-flow-verify-overview.md)
* [Další směrování](network-watcher-next-hop-overview.md)
* [Zobrazení skupin zabezpečení](network-watcher-security-group-view-overview.md)
* [Protokolování toku NSG](network-watcher-nsg-flow-logging-overview.md)
* [Řešení potíží virtuální síťová brána](network-watcher-troubleshoot-overview.md)

Po vytvoření instance sledovací proces sítě zachycení balíčku můžete nakonfigurovat taky v následujícím článku hello: [vytvořit zaznamenání výstrahy spouštěná paketu](network-watcher-alert-triggered-packet-capture.md)

[1]: ./media/network-watcher-create/figure1.png











